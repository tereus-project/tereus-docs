---
sidebar_position: 1
---

# How it works

Tereus transpilers mostly works like a compiler but instead of generating machine code, it generates text.

We use ANLTR to parse the source code into a AST (Abstract Syntax Tree).

> ANTLR (ANother Tool for Language Recognition) is a powerful parser generator for reading, processing, executing, or translating structured text or binary files.
>
> https://github.com/antlr/antlr4

## Architecture

mermaaaaid

## How to add a supported feature to the transpiler

Let's say we want to support the `continue` keyword for the c-to-go compiler.

We need to add the `continue` keyword to the `C.g4` grammar.

```antlr title="C.g4"
statement: (
		(
			variableDeclaration
			| expression
			| functionReturn
			| 'break'
// highlight-start
			| 'continue'
// highlight-end
			| structDeclaration
			| enumDeclaration
			| gotoStatement
		) ';'
	)
	| ifStatement
	| forStatement
	| whileStatement
	| block
	| labelStatement
	| BlockComment
	| LineComment;
```

```antlr title="C.g4"
Break: 'break';
Case: 'case';
Char: 'char';
Const: 'const';
// highlight-start
Continue: 'continue';
// highlight-end
Default: 'default';
```

Then, we will need to regerate our Go parser using ANTLR:

```sh
java -Xmx500M -cp "./bin/antlr-4.9-complete.jar" org.antlr.v4.Tool \
    -Dlanguage=Go \
    -no-listener \
    -visitor \
    -o parser \
    C.g4
```

We now have a have a bunch of updated files in the `/parser` directory, where the ANTLR generated code lies.

The next step is to handle the `continue` statement in the AST.

```go title="transpiler/ast/continue.go"
package ast

type ASTContinue struct{}

func NewASTContinue() *ASTContinue {
	return &ASTContinue{}
}
```

Then, we have to handle the `String()` method, which is used to generate the text representation of the AST.

```go title="transpiler/ast/continue.go"
package ast

type ASTContinue struct{}

func NewASTContinue() *ASTContinue {
	return &ASTContinue{}
}
// highlight-start
func (b *ASTContinue) String() string {
	return "continue"
}
// highlight-end
```

Then, we have to make sure that the `ASTContinue` is matched when visit a statement.

```go title="transpiler/visitor.go"
func (v *Visitor) VisitStatement(ctx *parser.StatementContext) (ast.IASTItem, error) {
	if child := ctx.VariableDeclaration(); child != nil {
		variableDeclaration, err := v.VisitVariableDeclaration(child.(*parser.VariableDeclarationContext))
		if err != nil {
			return nil, err
		}

		return variableDeclaration, nil
	} else if child := ctx.Expression(); child != nil {
		return v.VisitExpressionWithConfigurableIsStatement(child, true)
	} else if child := ctx.FunctionReturn(); child != nil {
		return v.VisitFunctionReturn(child.(*parser.FunctionReturnContext))
	} else if child := ctx.Break(); child != nil {
		return ast.NewASTBreak(), nil
// highlight-start
	} else if child := ctx.Continue(); child != nil {
		return ast.NewASTContinue(), nil
// highlight-end
	} else if child := ctx.StructDeclaration(); child != nil {

[...]
```

That's it!

To make sure our new `continue` keyword is supported, we can add some tests:

```go title="transpiler/transpiler_test.go"
func TestContinue(t *testing.T) {
	source := `
int main() {
	int a = 1;
	while (a < 10) {
		a++;
		if (a == 5) {
			continue;
		}
	}
	return 0;
}
`

	target := `
package main
import (
	"os"
)
func main() {
	a := 1
	for a < 10 {
		a++
		if a == 5 {
			continue
		}
	}
	os.Exit(0)
}
`

	testRemix(t, source, target)
}
```

And that's it!

```
» go test -v -run ^TestContinue$ github.com/tereus-project/tereus-transpiler-c-go/transpiler
=== RUN   TestContinue
--- PASS: TestContinue (0.01s)
PASS
ok      github.com/tereus-project/tereus-transpiler-c-go/transpiler     0.010s
```
