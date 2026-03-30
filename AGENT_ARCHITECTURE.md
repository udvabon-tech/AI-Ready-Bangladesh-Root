# Agent Architecture: Skills, MCP Servers & Orchestration

## Design Principle

The AI Ready Bangladesh autonomous system is built as a **hub-and-spoke model**:
- A **central orchestrator** (any Claude Code session with access to this repo) understands the full business
- **Specialized skills** handle domain-specific tasks without needing full context
- **MCP servers** provide real-time data access to external systems
- **Scheduled agents** handle recurring operations without human prompting

This architecture minimizes token usage by loading only the context needed for each task.

---

## MCP Servers (Data Access Layer)

MCP servers provide TOOLS for AI agents to interact with external systems. They are stateless data pipes — they don't contain business logic or decision-making.

### 1. `aireadymeta` — Meta Ads Manager
**Status:** ✅ Exists
**Purpose:** Read/write access to Meta Ads account (act_1290501775895152)
**Capabilities:**
- Get campaigns, ad sets, ads
- Get insights (account, campaign, ad set, ad level)
- Create/duplicate campaigns and ad sets
- Update budgets, pause/enable campaigns
- Upload ad images

**Used by:** Ad Operations skill, Marketing skill, Daily Report agent

### 2. `aiready-convex` — Business Database (NEEDED)
**Status:** ❌ To Build
**Purpose:** Direct access to Convex database for order management, CRM, and enrollments
**Capabilities needed:**
- Query book orders (by phone, status, date range)
- Query course orders (by user, payment status)
- Query enrollments (by user, check access)
- Update order status (confirm, cancel, flag)
- Query CRM data (customer aggregates)
- Read SMS logs
- Manage blocked phones

**Used by:** Customer Care skill, Operations skill, Daily Report agent

### 3. `aiready-steadfast` — Courier Management (NEEDED)
**Status:** ❌ To Build
**Purpose:** Create and track Steadfast deliveries
**Capabilities needed:**
- Create consignment for confirmed orders
- Check delivery status
- Get tracking info
- Bulk status queries

**Used by:** Operations skill, Customer Care skill

### 4. `aiready-messenger` — Customer Communication (NEEDED)
**Status:** ❌ To Build
**Purpose:** Send/receive messages on Facebook Messenger and WhatsApp
**Capabilities needed:**
- Read incoming messages
- Send replies (using Customer Care templates)
- Read Facebook comments on ads/posts
- Reply to comments

**Used by:** Customer Care skill

### 5. `aiready-facebook` — Social Media Management (NEEDED)
**Status:** ❌ To Build
**Purpose:** Publish and manage organic Facebook content
**Capabilities needed:**
- Create Facebook posts (text, image, video, link)
- Schedule posts
- Read post engagement metrics
- Read page insights

**Used by:** Content skill, Marketing skill

---

## Skills (Business Logic Layer)

Skills are **self-contained prompts with embedded business rules** that handle specific domains. They load only their relevant context files from this repo, keeping token usage minimal.

### Skill 1: `aiready-customer-care`
**Purpose:** Handle all customer interactions autonomously
**Context files loaded:**
- `CUSTOMER_CARE.md` (response templates, escalation rules)
- `BRAND_VOICE.md` (tone rules)
- `PRODUCTS.md` (pricing, availability)
**MCP servers used:** `aiready-convex`, `aiready-messenger`, `aiready-steadfast`
**Triggers:** Incoming Messenger message, WhatsApp message, Facebook comment
**Autonomous:** Yes — handles all standard queries. Escalates per DECISION_AUTHORITY.md

### Skill 2: `aiready-ad-ops`
**Purpose:** Monitor and manage Meta Ads campaigns
**Context files loaded:**
- `AD_OPERATIONS.md` (playbook, benchmarks, rules)
- `FINANCIALS.md` (budget constraints)
- `DECISION_AUTHORITY.md` (what needs approval)
**MCP servers used:** `aireadymeta`
**Triggers:** Daily scheduled check, anomaly alerts
**Autonomous:** Yes for adjustments within limits. Approval needed for major changes.

### Skill 3: `aiready-content-writer`
**Purpose:** Draft all content — ads, posts, emails, SMS, landing page copy
**Context files loaded:**
- `COPYWRITING.md` (templates, rules, forbidden patterns)
- `BRAND_VOICE.md` (tone, language, identity)
- `MARKETING.md` (content pillars, strategy)
**MCP servers used:** `aiready-facebook` (for publishing)
**Triggers:** Content calendar schedule, ad creative refresh needed, manual request
**Autonomous:** Drafts autonomously. Publishing requires review OR auto-publishes if following approved content calendar.

### Skill 4: `aiready-operations`
**Purpose:** Order processing, fulfillment, and operational monitoring
**Context files loaded:**
- `OPERATIONS.md` (fulfillment flows, team info)
- `PRODUCTS.md` (product details)
**MCP servers used:** `aiready-convex`, `aiready-steadfast`
**Triggers:** New order received, order status change, daily operations check
**Autonomous:** Yes for standard order flow. Flags edge cases.

### Skill 5: `aiready-analyst`
**Purpose:** Business intelligence — revenue tracking, ad performance analysis, customer insights
**Context files loaded:**
- `FINANCIALS.md` (benchmarks, unit economics)
- `AD_OPERATIONS.md` (performance thresholds)
- `GROWTH_ROADMAP.md` (goals and targets)
**MCP servers used:** `aireadymeta`, `aiready-convex`
**Triggers:** Weekly report schedule, manual request, anomaly detection
**Autonomous:** Read-only analysis. Recommendations go to Adnan.

### Skill 6: `aiready-bd-dev` (Existing)
**Purpose:** Development on the aiready.bd codebase
**Context files loaded:** Codebase at `/Users/Adnan/AdnanHQ/aiready.bd/`
**Triggers:** Bug reports, feature requests, technical issues
**Autonomous:** Code changes in worktree. Deployment requires approval.

---

## Scheduled Agents (Automation Layer)

These run on cron schedules without human prompting.

### Daily Morning Report (8:00 AM BST)
**Runs:** `aiready-analyst` + `aiready-ad-ops`
**Does:**
1. Pull yesterday's ad performance
2. Compare against benchmarks
3. Check for anomalies (CPA spikes, zero conversions, etc.)
4. Pull order/revenue summary
5. Generate brief report → send to Adnan via WhatsApp

### Daily Order Processing (Every 2 hours)
**Runs:** `aiready-operations`
**Does:**
1. Check for new pending orders
2. Run fraud checks on new orders
3. Confirm clean orders
4. Create Steadfast consignments
5. Process webhook updates from Steadfast

### Daily Customer Support (Continuous / Every 15 min)
**Runs:** `aiready-customer-care`
**Does:**
1. Check for unread Messenger messages
2. Check for unread WhatsApp messages
3. Check for unanswered Facebook comments on ads
4. Respond using templates and order data
5. Log interactions

### Weekly Content Planning (Sunday)
**Runs:** `aiready-content-writer` + `aiready-analyst`
**Does:**
1. Review last week's content performance
2. Draft next week's content calendar (4-5 posts)
3. Generate ad creative copy if refresh needed
4. Submit plan to Adnan for approval

### Monthly Performance Review (1st of month)
**Runs:** `aiready-analyst`
**Does:**
1. Full month ad performance summary
2. Revenue and profit calculation
3. Customer growth metrics
4. Comparison against Growth Roadmap targets
5. Recommendations for next month

---

## How It All Fits Together

```
                    ┌─────────────────────────┐
                    │     ADNAN (Founder)      │
                    │  Vision / Strategy /     │
                    │  Content Recording       │
                    └────────────┬────────────┘
                                 │ Approvals & Direction
                                 ▼
              ┌──────────────────────────────────────┐
              │        ORCHESTRATOR                   │
              │  (Claude Code + This Repo)            │
              │  Reads: All .md files as needed       │
              │  Coordinates skills & agents          │
              └───┬────┬────┬────┬────┬────┬────────┘
                  │    │    │    │    │    │
         ┌────────┘    │    │    │    │    └────────┐
         ▼             ▼    ▼    ▼    ▼             ▼
   ┌──────────┐  ┌────┐┌────┐┌────┐┌────┐   ┌──────────┐
   │Customer  │  │ Ad ││Con-││Ops ││Ana-│   │   Dev    │
   │  Care    │  │Ops ││tent││    ││lyst│   │(aiready- │
   │  Skill   │  │    ││    ││    ││    │   │ bd-dev)  │
   └────┬─────┘  └─┬──┘└─┬──┘└─┬──┘└─┬──┘   └────┬─────┘
        │          │     │     │     │            │
        ▼          ▼     ▼     ▼     ▼            ▼
   ┌─────────────────────────────────────────────────────┐
   │              MCP SERVERS (Data Layer)                │
   │                                                     │
   │  aireadymeta    aiready-convex    aiready-steadfast  │
   │  (Meta Ads)     (Database)        (Courier)         │
   │                                                     │
   │  aiready-messenger    aiready-facebook              │
   │  (Chat/WhatsApp)      (Social Media)                │
   └─────────────────────────────────────────────────────┘
```

## Token Efficiency Design

| Approach | Why |
|----------|-----|
| Skills load only their 1-2 relevant .md files | Not the whole business bible |
| MCP servers return structured data | Not raw HTML or verbose responses |
| Scheduled agents are single-purpose | One job, minimal context |
| Orchestrator delegates, doesn't do everything | Dispatches to the right skill |
| Customer care templates are pre-written | Agent fills in variables, not composing from scratch |
| Ad operations uses decision matrix | Lookup table, not reasoning from first principles each time |

## Build Priority

| # | What | Effort | Impact | Build Order |
|---|------|--------|--------|-------------|
| 1 | `aiready-customer-care` skill | Medium | High — replaces Hiron, improves response time | First |
| 2 | `aiready-convex` MCP server | Medium | High — needed by customer care + operations | First (parallel) |
| 3 | `aiready-ad-ops` skill | Low | High — daily monitoring is critical | Second |
| 4 | `aiready-messenger` MCP server | Medium | High — needed for customer care automation | Second (parallel) |
| 5 | `aiready-operations` skill | Medium | Medium — order processing automation | Third |
| 6 | `aiready-steadfast` MCP server | Low | Medium — needed by operations skill | Third (parallel) |
| 7 | `aiready-content-writer` skill | Medium | Medium — content creation support | Fourth |
| 8 | `aiready-facebook` MCP server | Medium | Medium — needed for content publishing | Fourth (parallel) |
| 9 | `aiready-analyst` skill | Low | Medium — reporting and insights | Fifth |
| 10 | Scheduled agents (cron) | Low each | High cumulative — full autonomy | After skills are ready |

## What Exists Today vs. What's Needed

| Component | Status |
|-----------|--------|
| This repo (business docs) | ✅ Created now |
| `aireadymeta` MCP server | ✅ Exists |
| `aiready-bd-dev` skill | ✅ Exists |
| `aiready-convex` MCP server | ❌ Need to build |
| `aiready-steadfast` MCP server | ❌ Need to build |
| `aiready-messenger` MCP server | ❌ Need to build |
| `aiready-facebook` MCP server | ❌ Need to build |
| `aiready-customer-care` skill | ❌ Need to build |
| `aiready-ad-ops` skill | ❌ Need to build |
| `aiready-content-writer` skill | ❌ Need to build |
| `aiready-operations` skill | ❌ Need to build |
| `aiready-analyst` skill | ❌ Need to build |
| Scheduled agents | ❌ Need to configure |
