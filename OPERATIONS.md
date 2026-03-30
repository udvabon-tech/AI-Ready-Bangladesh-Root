# Operations Manual

## Team

| Person | Role | Compensation | Notes |
|--------|------|-------------|-------|
| **Abid Adnan** | Founder, strategist, content creator, developer | Owner | Makes all strategic decisions. Day job: Head of AI & Agents at OnnoRokom |
| **Mamun** | Book distribution & fulfillment | ৳50/book (now ৳40 after Morshed) | Handles physical logistics |
| **Morshed** | Distribution assistant | ৳10/book (from Mamun's share) | More capable than Mamun. Helping with distribution |
| **Hiron Vai** | Customer support | TBD | Adnan's cousin. Handles customer queries but not organized/overseen well |

### Team Notes
- Team is minimal. Adnan does most thinking/strategy/content/dev work.
- Customer support is being planned for **full AI automation** via Claude API with aireadymeta MCP server access.
- Goal: Replace manual team roles with AI automation where possible. Mamun/Morshed/Hiron roles should be systematized so AI can eventually manage or replace these functions.

## Order Fulfillment Flow — Books

```
Customer places order on website
    → Order created (status: pending)
    → Phone blocklist check (fraud prevention)
    → FraudBD API check (background)
    → If clean: status → confirmed
    → If risky: status → flagged (manual review)
    → On confirm: Steadfast consignment auto-created
    → Mamun/Morshed handle physical packing & handoff
    → Steadfast picks up and delivers
    → Webhook updates: in_review → pending → delivered/cancelled
    → ~10% return rate (COD reality)
```

## Order Fulfillment Flow — Courses

```
Customer visits course page
    → Chooses solo course or bundle
    → Guest checkout (pay first, register after) OR authenticated checkout
    → EPS payment gateway (bKash/Nagad/Rocket/Card)
    → Payment verified → order marked "paid"
    → Auto-enrolled in course (180-day access)
    → SMS confirmation sent
    → If bundle: book order also created (pending fulfillment)
```

## Tools & Platforms

| Tool | Purpose | Access |
|------|---------|--------|
| aiready.bd (Next.js + Convex) | Main website, course platform, admin dashboard | Code at `/Users/Adnan/AdnanHQ/aiready.bd/` |
| aiready.com.bd | Legacy site — one active book campaign | Separate codebase |
| Meta Ads Manager | Customer acquisition | Via aireadymeta MCP server |
| Steadfast (Packzy) | Courier & delivery tracking | API integrated |
| EPS Payment Gateway | Online payments | API integrated (sandbox + live) |
| FraudBD | Fraud detection | API integrated |
| Cloudflare R2 | File/image storage | API integrated |
| Facebook Page | Social media presence | https://www.facebook.com/aireadybd |
| Gmail | Business email | aireadybangladesh@gmail.com |
| WhatsApp (Adnan) | Founder communication | 01815358335 |
| WhatsApp (Business) | Customer communication (on site) | 01630300896 |
| IFIC Bank | Collections | Personal account |
| RedotPay | USD spending (ads) | Loaded at ৳129.5/USD |

## Admin Dashboard

Located at `aiready.bd/admin` (role-protected):
- **Orders:** View, filter, confirm, flag, cancel book orders
- **Course Orders:** View course purchases, payment status
- **CRM:** Customer database aggregated across all order types
- **Team:** Admin role management (super_admin, admin)
- **SMS Logs:** Outbound message audit trail

## Daily Operations Checklist

1. **Check ad performance** — Review Meta campaigns via MCP. Watch CPA and ROAS.
2. **Process pending orders** — Confirm clean orders, review flagged ones.
3. **Monitor delivery status** — Check Steadfast webhook updates.
4. **Handle customer queries** — Respond via Messenger/WhatsApp (to be automated).
5. **Track revenue** — Monitor course enrollments and book order completions.

## Known Issues

### Refund Policy Inconsistency (NEEDS FIX)
- `/refund-policy` page says: "No refunds or replacements" (with 24-hour exception for damaged items)
- `/terms` page says: Physical products refundable within 7 days if damaged/defective; Digital products within 3 days if access not received
- **Action needed:** Align these two pages. Recommend using the `/terms` version as it's more customer-friendly and legally protective.

---

## Sync Between Legacy & New Site

Orders from aiready.com.bd sync to aiready.bd via `/api/sync/orders`:
- `processing` → `pending`
- `completed` → `confirmed`
- `cancelled` → `cancelled`
- Deduplicated by `sourceId`
