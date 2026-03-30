# Decision Authority Matrix

## Principle

AI agents operating AI Ready Bangladesh should act with **maximum autonomy on routine operations** and **maximum caution on irreversible or brand-defining decisions**.

Adnan's time is best spent on vision, strategy, and content creation — not on approving order confirmations or adjusting ad budgets by ৳500.

---

## Fully Autonomous (No Approval Needed)

### Customer Support
- ✅ Answer all standard customer queries (order status, pricing, how-to)
- ✅ Process order cancellations (pre-shipment)
- ✅ Handle refund requests per existing refund policy
- ✅ Send order confirmations and updates
- ✅ Respond on Messenger, WhatsApp, Email, Comments
- ✅ Upsell book buyers to course (gently, per brand voice)

### Ad Operations
- ✅ Monitor campaign performance daily
- ✅ Adjust budgets within ±20% per day on existing campaigns
- ✅ Pause underperforming ad sets (CPA > ৳200 for 2+ days)
- ✅ Enable paused ad sets that were previously performing well
- ✅ Duplicate winning ads to test variations
- ✅ Create new campaigns (they start PAUSED by default)
- ✅ Total daily ad spend up to ৳15,000

### Operations
- ✅ Confirm clean orders (no fraud flags)
- ✅ Review and act on flagged orders using FraudBD data
- ✅ Create Steadfast consignments for confirmed orders
- ✅ Sync orders between legacy and new platform
- ✅ Monitor delivery status and update customers

### Content (with Brand Voice compliance)
- ✅ Draft Facebook posts following content pillars
- ✅ Draft ad copy following copywriting guidelines
- ✅ Draft email communications
- ✅ Write customer support responses
- ✅ Create course descriptions and sales page copy

### Technical
- ✅ Bug fixes on aiready.bd that don't change business logic
- ✅ Performance optimizations
- ✅ Pixel/CAPI health monitoring and fixes
- ✅ Database queries for customer support

---

## Requires Adnan's Approval

### Strategy & Brand
- 🔒 Launching a new product or course
- 🔒 Setting or changing product prices
- 🔒 Changing brand positioning or messaging direction
- 🔒 Public statements about company direction
- 🔒 Partnerships or collaboration agreements

### Financial
- 🔒 Total daily ad spend exceeding ৳15,000
- 🔒 Any new recurring cost or subscription
- 🔒 Payments to vendors or team members
- 🔒 Changes to payment gateway configuration (sandbox ↔ live)

### Ads (High-Risk)
- 🔒 Turning OFF a winning campaign entirely
- 🔒 Changing targeting on a proven winning campaign
- 🔒 Editing creative on a live winning ad (duplicate instead)
- 🔒 Budget increase > 20% in a single day on any campaign
- 🔒 Launching a campaign for a product that doesn't exist yet

### People
- 🔒 Hiring, firing, or changing team compensation
- 🔒 Assigning new responsibilities to Mamun, Morshed, or Hiron
- 🔒 Any communication sent AS Adnan (not as AI Ready Bangladesh)

### Technical (High-Risk)
- 🔒 Changing database schema
- 🔒 Modifying payment processing logic
- 🔒 Deploying to production (Convex deploy + Vercel)
- 🔒 Changing authentication or authorization logic
- 🔒 Modifying fraud detection thresholds

### Legal
- 🔒 Responding to legal threats
- 🔒 Changing terms of service, privacy policy, or refund policy
- 🔒 Any government or regulatory communication

---

## Emergency Authority

In case of emergencies, AI agents MAY act without approval:

| Emergency | Allowed Action |
|-----------|---------------|
| Website is down | Restart services, rollback deploy |
| Payment gateway broken | Switch to sandbox, notify customers of delay |
| Pixel/CAPI broken | Fix immediately (conversion data loss = revenue loss) |
| Ad spend runaway (CPA > ৳300) | Pause the campaign immediately |
| Security breach detected | Lock down affected systems, notify Adnan ASAP |
| Fraudulent orders flooding in | Enable stricter fraud checks, block phone numbers |

**After any emergency action:** Immediately notify Adnan with what happened, what was done, and why.

---

## Communication Protocol

When an AI agent needs Adnan's approval:
1. Send a concise summary to WhatsApp (01815358335)
2. Include: What needs approval, why, recommended action, deadline for decision
3. If no response within 24 hours on non-urgent matters: proceed with the safest option
4. If no response within 2 hours on urgent matters: take the conservative action and explain later
