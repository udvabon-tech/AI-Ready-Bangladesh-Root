# Technical Infrastructure Reference

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                    aiready.bd                        │
│              (Next.js 15 + React 19)                 │
│                  Hosted on Vercel                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Public Pages          Dashboard          Admin     │
│  ├─ /                  ├─ /dashboard      ├─ /admin │
│  ├─ /books/ai-ready    ├─ /courses        ├─ /orders│
│  ├─ /courses/[slug]    ├─ /profile        ├─ /crm   │
│  ├─ /aireadybundle     └─ /credits        └─ /team  │
│  └─ /track                                          │
│                                                     │
├─────────────────────────────────────────────────────┤
│                  API Routes                         │
│  ├─ /api/payment/eps/* (initiate, verify, ipn)      │
│  ├─ /api/meta/capi (Conversions API)                │
│  ├─ /api/fraud-check (FraudBD)                      │
│  ├─ /api/steadfast/webhook (delivery tracking)      │
│  ├─ /api/sync/orders (legacy sync)                  │
│  └─ /api/upload (R2 file upload)                    │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│              Convex (Backend)                        │
│         dev:silent-gecko-681                         │
│  https://silent-gecko-681.convex.cloud               │
├─────────────────────────────────────────────────────┤
│  Tables:                                            │
│  ├─ bookOrders (physical book orders)               │
│  ├─ courses (course catalog)                        │
│  ├─ courseLessons (video lessons)                   │
│  ├─ courseOrders (digital purchases)                 │
│  ├─ courseEnrollments (access control)               │
│  ├─ blockedPhones (fraud blocklist)                 │
│  ├─ adminRoles (team permissions)                   │
│  ├─ smsLogs (outbound SMS audit)                    │
│  └─ authTables (users, sessions)                    │
└─────────────────────────────────────────────────────┘

External Services:
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│  EPS Gateway │ │  Steadfast   │ │  FraudBD     │
│  (Payments)  │ │  (Courier)   │ │  (Fraud Det.)│
└──────────────┘ └──────────────┘ └──────────────┘
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ Cloudflare R2│ │  Meta Pixel  │ │  SMS Service │
│ (Storage)    │ │  + CAPI      │ │              │
└──────────────┘ └──────────────┘ └──────────────┘
```

## Codebase Locations

| Project | Path | Purpose |
|---------|------|---------|
| aiready.bd | `/Users/Adnan/AdnanHQ/aiready.bd/` | Main website + platform |
| ihhya.com / Khushu | `/Users/Adnan/AdnanHQ/ihhya khushu unicorn/` | Islamic products venture |
| AI Ready Bangladesh Root | `/Users/Adnan/AI Ready Bangladesh Root/` | Business operations docs (this repo) |

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15, React 19, TypeScript 5, Tailwind CSS v4 |
| Backend | Convex 1.32 (serverless functions + real-time DB) |
| Auth | Convex Auth (Password + Google OAuth) |
| Hosting | Vercel (frontend + API routes), Convex Cloud (backend) |
| Storage | Cloudflare R2 (bucket: `aiready-storage`) |
| Payment | EPS (Easy Payment System) — bKash, Nagad, Rocket, Cards |
| Courier | Steadfast (portal.packzy.com) |
| Fraud | FraudBD API |
| Analytics | Meta Pixel + Conversions API (Pixel ID: 2316357295553298) |
| Package Manager | pnpm |

## Key Environment Variables

| Variable | Purpose |
|----------|---------|
| CONVEX_DEPLOYMENT | Convex deployment identifier |
| NEXT_PUBLIC_CONVEX_URL | Public Convex endpoint |
| EPS_MODE | Payment gateway mode (sandbox/live) |
| EPS_MERCHANT_ID | EPS merchant identifier |
| META_PIXEL_ID | Facebook Pixel ID |
| META_ACCESS_TOKEN | Conversions API token |
| R2_ACCOUNT_ID | Cloudflare R2 account |
| STEADFAST_API_KEY | Courier API key |
| FRAUDBD_API_KEY | Fraud detection API key |

## Deployment

**Two-step production deployment:**
```bash
# Step 1: Deploy Convex backend
npx convex deploy --cmd 'echo ok' --yes

# Step 2: Deploy Next.js frontend
vercel --prod --yes
```

**Local development:**
```bash
pnpm dev                    # Next.js dev server (port 3000)
npx convex dev --once       # Convex local functions
```

## Database Schema Summary

### bookOrders
- Core: name, phone, address, area, quantity
- Pricing: bookPrice, deliveryCharge, totalPrice
- Status: pending → confirmed → delivered/cancelled/flagged
- Fraud: fraudData, fraudCheckedAt
- Delivery: steadfastConsignmentId, steadfastTrackingCode, steadfastStatus
- Sync: source (aiready.bd/aiready.com.bd), sourceId

### courses
- Core: slug, title, subtitle, description, price, thumbnailUrl
- Instructor: name, title, image
- Meta: duration, lessonsCount, accessDays (180)
- Status: active/draft

### courseOrders
- Core: userId (optional), courseId, amount
- Payment: paymentStatus (pending/paid/failed/refunded), gateway, transactionId
- Customer: phone, name, address (for bundles)

### courseEnrollments
- Core: userId, courseId, enrolledAt, expiresAt, status (active/expired)
- Access check: enrollment exists + status active + not expired

## Integration Details

### EPS Payment Gateway
- Sandbox: merchant `29e86e70-0ac6-45eb-ba04-9fcb0aaed12a`
- Live: merchant `e741024e-15f7-45f5-9920-aba0ce8f448c`
- Token caching with auto-refresh
- IPN: AES-256-CBC encrypted webhooks
- Hash: HMAC-SHA512 authentication

### Steadfast Courier
- Portal: `https://portal.packzy.com/api/v1`
- Auto-creates consignment on order confirm
- Webhook receives: in_review, pending, delivered, cancelled, hold
- Tracking code displayed to customers

### Meta Pixel + CAPI
- Dual tracking: browser Pixel + server CAPI
- Events: PageView, ViewContent, InitiateCheckout, Purchase, Lead
- Deduplication via shared event IDs
- User data hashed (SHA-256) before sending to Meta

### FraudBD
- API: `https://fraudbd.com/api/check-courier-info`
- Checks: courier cancel rates, Pathao ratings
- Risk threshold: > 50% cancel rate with 3+ orders = flagged
- Runs as background job (non-blocking)

## Development Practices

- **Spec-driven:** All features need a spec before coding (in `specs/<feature>/`)
- **Worktree-based:** Feature work in git worktrees, never direct main commits
- **TypeScript strict mode**
- **One PR per feature**
- **Bugfixes exempt from spec requirement**
