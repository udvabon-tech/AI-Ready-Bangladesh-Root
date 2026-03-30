# Meta Ads Operations Playbook

## Access

AI agents can access the Meta Ads account via the **aireadymeta MCP server**.
- **Ad Account ID:** act_1290501775895152
- **Currency:** BDT (Bangladeshi Taka)
- **All new campaigns start PAUSED** — this is a safety default.

## Campaign Structure (Current)

```
Account: act_1290501775895152
├── AIREADY_BOOK_SCALING_CBO (Active, ৳4,500/day) — Main book sales driver
├── AiReady_Book_ABO (Active) — Book ad testing
├── ABID_ADNAN_ABO (Active) — Personal brand authority
├── BUNDLE_ASC_V1 - Copy (Active, ৳2,000/day) — Bundle sales
└── MICROCOURSE_UPSELL_CBO (Paused, ৳1,500/day) — Course upsell
```

## Daily Ad Management Routine

### Morning Check (Every Day)

1. **Pull account insights** for `yesterday` and `last_7d`
2. **Check each active campaign:**
   - Is CPA within healthy range (< ৳150 for books)?
   - Is ROAS above 3.5x?
   - Is frequency below 3.0?
3. **Check for anomalies:**
   - Sudden spend spikes
   - Zero conversions with significant spend
   - Unusually high CPM

### Decision Matrix

| Situation | Action |
|-----------|--------|
| CPA < ৳130 for 3+ days, ROAS > 4x | Scale budget +20% (max once per day) |
| CPA ৳130–150, stable | Hold. Monitor. |
| CPA ৳150–200 | Review ad set performance. Pause worst performers. |
| CPA > ৳200 for 2+ days | Pause underperforming ad sets. Test new creatives. |
| Frequency > 4.0 | Audience fatigue. Expand targeting or refresh creatives. |
| CTR drops below 2% | Creative fatigue. Need new ad content. |
| ROAS < 2.5x for 3+ days | Reduce budget. Diagnose issue (creative? audience? landing page?). |
| Zero purchases in a day with spend > ৳2,000 | Check pixel/CAPI. Check landing page. Check payment gateway. |

## Rules for Winning Campaigns

> **CRITICAL: Winning campaigns must be treated with extreme caution.**

1. **Never turn off a profitable campaign** without explicit reason.
2. **Never change multiple variables at once** on a winning campaign.
3. **Budget changes:** Maximum 20% increase per day. Never decrease by more than 20%.
4. **Do not edit winning ad creatives.** Duplicate and test variations instead.
5. **Do not change targeting** on a winning campaign. Create a new ad set to test.
6. **If a winning campaign's performance dips:** Wait 3 days before taking action. Meta's algorithm fluctuates.

## Testing New Campaigns

### Process

1. **Create campaign PAUSED** (MCP default)
2. **Structure:** Objective = OUTCOME_SALES
3. **Budget:** Start at ৳1,000–1,500/day for testing
4. **Ad Sets:** 2–3 variations (different audiences or placements)
5. **Ads:** 2–3 creative variations per ad set
6. **Run for 5–7 days** before making judgments
7. **Kill losers, scale winners** — but give Meta's algorithm time to learn

### When Testing New Products (Courses, Bundles)

- Start with **ABO** (Ad Set Budget Optimization) to control spend per audience
- Once a winner is found, graduate to **CBO** for scaling
- Use **ASC (Advantage+ Shopping Campaign)** for proven products with broad appeal

## Campaign Naming Convention

```
{PRODUCT}_{AUDIENCE/TYPE}_{OPTIMIZATION}_{VERSION}
```

Examples:
- `AIREADY_BOOK_SCALING_CBO` — Main book, scaling phase, CBO
- `BUNDLE_ASC_V1` — Bundle product, Advantage+ Shopping, version 1
- `MICROCOURSE_UPSELL_CBO` — Micro course, upsell audience, CBO
- `ABID_ADNAN_ABO` — Personal brand content, ABO for testing

## Budget Guidelines

| Campaign Type | Daily Budget Range |
|--------------|-------------------|
| Proven scaling campaign | ৳3,000–6,000 |
| Testing new creative/audience | ৳1,000–1,500 |
| New product launch test | ৳1,500–2,000 |
| Retargeting / upsell | ৳1,000–2,000 |
| **Total daily max (without Adnan's approval)** | **৳15,000** |

If total daily spend needs to exceed ৳15,000 — seek Adnan's approval.

## Audience Insights

- **Primary:** Bangladesh-wide, 18–45 age range
- **Interest signals:** Technology, education, entrepreneurship, freelancing
- **Best performing:** Broad targeting with Meta's AI optimization (let the algorithm find buyers)
- **Retargeting:** Website visitors, video viewers, page engagers (use for upsell campaigns)

## Creative Best Practices

1. **Video > Image** — Adnan speaking to camera performs best
2. **First 3 seconds:** Must hook with a bold statement about AI / Bangladesh's future
3. **Length:** 30–90 seconds for feed. 15–30 for stories/reels.
4. **Subtitles:** Always. Most watch without sound.
5. **Language:** Bangla with English AI terms
6. **No text-heavy graphics** — Meta penalizes them in auction

## Pixel & CAPI Health Checks

Monthly:
1. Verify pixel fires on all key pages (ViewContent, InitiateCheckout, Purchase)
2. Verify CAPI events are sending with matching event IDs
3. Check Event Manager for deduplication working correctly
4. Ensure purchase values are being reported accurately in BDT

If pixel/CAPI breaks, this is a **P0 emergency** — Meta's algorithm depends on accurate conversion data.
