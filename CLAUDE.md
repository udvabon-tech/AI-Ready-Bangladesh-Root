# AI Ready Bangladesh — Agent Instructions

This is the business operations root for AI Ready Bangladesh. Read the relevant `.md` files before taking any action.

## Available MCP Servers

- **aiready-convex** — Database access (orders, customers, enrollments, fraud). Uses `npx convex run` under the hood.
- **meta-ads** — Meta Ads Manager (campaigns, insights, budgets). Ad account: act_1290501775895152.

## Available Skills

- **aiready-customer-care** — Handle customer support queries using Convex data + brand voice templates.

## Key Rules

1. Read `DECISION_AUTHORITY.md` before taking any action — it defines what you can do autonomously vs what needs Adnan's approval.
2. Read `BRAND_VOICE.md` before writing any customer-facing content.
3. Read `AD_OPERATIONS.md` before touching any campaigns — winning campaigns are sacred.
4. Book price (৳499) is NEVER discounted. Course is ৳790. Bundle is ৳990.
5. All customer communication in Bangla (Bengali script), English for tech terms only.

## Quick File Reference

| Need to... | Read this |
|------------|-----------|
| Understand the business | BUSINESS_OVERVIEW.md |
| Write copy or content | COPYWRITING.md + BRAND_VOICE.md |
| Handle customer support | CUSTOMER_CARE.md |
| Manage ads | AD_OPERATIONS.md |
| Check pricing/products | PRODUCTS.md |
| Make a decision | DECISION_AUTHORITY.md |
| Understand the tech | TECH_INFRASTRUCTURE.md |

## Codebase

The aiready.bd website code is at `/Users/Adnan/AdnanHQ/aiready.bd/` (Next.js 15 + Convex).
