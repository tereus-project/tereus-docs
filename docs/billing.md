---
sidebar_position: 2
---

# Pricing and Billing

## Tiers

Tereus currently has 3 billing plans:

| **Tier**           | Free | Pro              | Enterprise            |
| ------------------ | ---- | ---------------- | --------------------- |
| **Inline**         | ✅   | ✅               | ✅                    |
| **Zip**            | ❌   | <= 1MB           | ✅                    |
| **Git**            | ❌   | ❌               | ✅                    |
| **Data retention** | 24h  | $0.2/Mo          | 2GB free then $0.1/Mo |
| **Support**        | ❌   | Only with issues | Full support          |
| **Cost**           | Free | $49/month        | $279/month            |

### Free tier

The free tier is designed to be used for personal use and for testing the service. We require that you sign using your Github or Gitlab account to get access to the free tier, in order to prevent abuse.

:::info Retention
The free tier has a 24h retention policy, meaning that the data will be deleted after 24 hours if you don't upgrade to a paid plan.
:::

### Pro tier

The Pro tier is designed to be used for small teams and for large teams that want to use the service in a more cost-effective way.

### Enterprise tier

The Enterprise tier is designed for teams that want to use the service for big projects and want full support from the Tereus team.

We plan on adding automation support for this tier through the API, which will allow you to integrate Tereus into your own systems and applications. A use case could be to automatically convert your project using a CI system.

## How to manage your plan

Once logged-in, you can go to https://tereus.io/pricing to manage your plan. If you upgrade your plan, you will be charged the difference between your current plan and the new plan.

## Billing

:::tip Info
The billing process is handled by Stripe. You can find more information about Stripe [here](https://stripe.com/docs/billing/subscriptions).
:::

The subscription is a flat rate, so you will be charged the same amount every month for the following month.
However, we also charge you data usage on a per-byte basis, so you will be charged a different amount for each month depending on how much data you use.
