# Telecom Tycoon — How It Works

A full breakdown of every game mechanic. Use this as a reference when building a new version.

---

## The Game Loop

The game runs on a **tick system**. Every 100ms in real time = 1 tick. 240 ticks = 1 game-day. So one game-day passes every 24 real seconds.

Every tick, the game:
1. Generates bandwidth (if you have suppliers)
2. Earns revenue from customers
3. Advances the router manufacturer
4. Grows LCO customer potential
5. Drains data inventory from customers consuming it
6. Checks for unlocks
7. Re-renders the UI

---

## Resources

| Resource | What it is |
|---|---|
| **Funds ($)** | Your money. Earned from customers, spent on everything. |
| **Data Inventory (GB)** | Your stock of bandwidth. Customers consume it; suppliers generate it. If it hits 0, customer sentiment drops. |
| **Routers** | Physical hardware. One router = one customer. You need them to onboard customers. |

---

## Clicking (Early Game)

The **Generate Data** button gives you +1 GB per click. It costs money to generate (based on your cheapest supplier's rate). This is how you bootstrap before you have any automation.

Click cost formula:
- Starts at ~$0.01/GB
- Gets cheaper as you upgrade suppliers
- Has a daily random noise multiplier (±20%) to simulate market fluctuation

---

## Hardware Manufacturing

Unlocks at **50 GB** in inventory.

The manufacturer produces routers automatically. You can also order them instantly (from the manufacturer's stock) or queue them for production.

| Order | Cost | Discount |
|---|---|---|
| 1 router | $5 | — |
| 5 routers | $22 | 12% |
| 25 routers | $90 | 28% |

- The production line has levels. Higher levels = faster production + more stock capacity.
- Upgrade cost scales up with each level (~×2.5 per level).

---

## LCOs (Local Cable Operators)

Unlocks when you have **3+ routers** in stock.

LCOs are your distribution partners — they bring you customers in a specific region. Each LCO has:

- **Region** — Rural, Suburban, Urban, or Metro. Affects demand, commission rate, and how much data customers use.
- **Customers** — integer count of active subscribers.
- **Potential** — the ceiling of how many customers this LCO can have. Grows over time toward a target.
- **Sentiment** — 0–100. Low sentiment = more churn (customers leaving). Drops when data runs out or prices are too high.
- **Subscription price** — you set this. Too high = low demand. Too low = less revenue.

### How customers grow

Each tick, the LCO's **potential** drifts upward toward a target. When potential exceeds the current customer count AND you have spare routers, customers are automatically onboarded (one router deployed per new customer).

Growth target formula: `12 × 2^(level-1) × region_acquisition_multiplier`

Growth speed depends on: region demand, sentiment, infra/app upgrades, and Telecom Token bonuses.

### Churn

Every game-day, some customers leave. Churn is higher when:
- Sentiment is low
- Price is above the region's target price
- Data inventory ran dry

### LCO Upgrades

| Upgrade | Effect |
|---|---|
| Install Crew | +5 to potential cap |
| Expansion (Fibre) | +10 to potential cap |
| Customer Support | +8 sentiment |
| ONU/OLT Infra | +20% growth speed |
| Customer App | +4 sentiment, +10% growth speed |
| OTT Add-on | +$0.50/month per customer (bundled revenue) |
| Upgrade Level | ×2 potential cap, faster growth |

### Revenue

Each LCO earns: `subscription_price × customers × (1 - commission_rate) ÷ 30` per game-day.

Commission rates by region:
- Rural: 5%
- Suburban: 10%
- Urban: 15%
- Metro: 25%

---

## Bandwidth Suppliers

Unlocks after your **first LCO partnership**.

Suppliers auto-generate GB every game-day so you don't have to keep clicking. Each supplier has:

- **Level** — determines speed tier, generation rate, and per-GB cost.
- **Uptime** — ~96% by default. On failure, the supplier goes offline for 1–4 game-hours.
- **Generation** — starts at 5 GB/day (Level 1), scales ×2.5 per level (L2=12.5, L3=31, L4=78, L5=195...).

### Supplier Upgrades

| Upgrade | Effect |
|---|---|
| Upgrade Tier | ×2.5 generation, higher speed |
| Procurement | −2% per-GB cost |
| Network Ops (NOC) | +0.5% uptime |
| Capacity Engineering | +10% generation |
| Supplier Backup | −25% outage duration |

You can also **buy bandwidth on the spot** (10,000 GB at market rate) without a supplier.

---

## Open Marketplace

Unlocks alongside bandwidth suppliers.

- **Sell data** — liquidate 1,000 GB for ~50% of market rate (quick cash)
- **Sell routers** — liquidate 5 routers for ~50% of cost
- **Active listings** — discounted lots occasionally appear (random events). Limited time to buy.
- **Auto-sell** — toggle to automatically sell data above a threshold you set

---

## Random Events

After unlocks, random events fire roughly once every half game-day. Examples:

| Event | Effect |
|---|---|
| Supplier price surge | Click cost goes up 2–3× for the day |
| Fibre cut | One LCO goes offline for a few game-hours |
| Customer complaints | One LCO loses sentiment |
| Word of mouth | Growth speed +20% for a game-day |
| Festival | Data consumption +50% for a game-day |
| Government grant | +$10 free money |
| Discounted lot | 2 cheap listings appear on the marketplace |

---

## Milestones

Triggered by total customer count. One-time bonuses:

| Customers | Reward |
|---|---|
| 10 | +$30 cash |
| 100 | LCO acquisition speed +20% |
| 500 | LCO acquisition speed +10% |
| 1,000 | Bandwidth generation +20% |
| 5,000 | Router efficiency +50% |

---

## Prestige

Unlocks at **10,000 customers**.

Prestige resets the run (funds, customers, LCOs, suppliers) but carries forward **Telecom Tokens (TT)** — a permanent meta-currency.

Tokens earned = `√(lifetime_customers ÷ 100)`

### Spending Telecom Tokens (persist across runs)

| Track | Effect | Max |
|---|---|---|
| Hardware Efficiency | Mfg speed +3% per TT | 25 |
| LCO Efficiency | Growth speed +3% per TT | 25 |
| Bandwidth Efficiency | Generation +3% per TT | 25 |
| Reliability | Failure chance ×0.95 per TT | 10 |
| Capital | Starting funds +$100 per TT | 20 |

Each subsequent run is faster because your upgrades carry over.

---

## Regions

| Region | Avg GB/customer/day | Commission | Target price/mo | Acquisition mult |
|---|---|---|---|---|
| Rural | 5 GB | 5% | $4.00 | 0.6× |
| Suburban | 8 GB | 10% | $6.00 | 1.0× |
| Urban | 12 GB | 15% | $9.00 | 1.5× |
| Metro | 18 GB | 25% | $14.00 | 2.0× |

---

## Speed Tiers (Subscription Price Cap)

Your maximum subscription price is capped by your best supplier's speed tier:

| Tier | Max price/mo |
|---|---|
| 100 Mbps | $7.50 |
| 250 Mbps | $10.00 |
| 1 Gbps | $15.00 |
| 2.5 Gbps | $20.00 |
| 10 Gbps | $25.00 |
| 25 Gbps | $30.00 |

---

## Key Design Patterns (for builders)

- **Everything is per-game-day**, then divided by `TICKS_PER_DAY` (240) to smooth it per tick. No sudden jumps.
- **Customers are integers; potential is a float** that drifts upward. This creates a natural growth feel.
- **The UI uses persistent DOM card caches** — cards are created once and updated in place, not re-rendered every tick. This keeps it fast.
- **All state lives in one `state` object** — easy to save/load with `JSON.stringify`.
- **No external libraries** — pure vanilla JS, no framework, no build step needed.
