# ACTUS-DRAPS
### Dynamic Risk-Adaptive Portfolio Swaps for Supply Chain Finance

> **ACTUS Competition 2025 · Trade Finance Risk Innovation**

## 🎬 Watch the Demo

**[▶ Click here to watch the full demo video](https://drive.google.com/file/d/1YkNdJ0d5ow6bu4e3XHPO5hdRU4fvrw-9/view)**

> See DRAPS in action — a real $1M India-US trade finance loan, 3 hedging scenarios, live ACTUS simulation output, and automatic winner analysis. No setup needed to watch.

---

## 🚀 Try It Yourself (Judges)

**You need:** Claude Desktop (free) + the JSON file in this repo. That's it.

1. Download **[SWAPS-1LOAN-WHAT-IF-DEMO.json](https://github.com/24pba027-droid/Swaps-for-Supply-Chain-Finance/blob/main/SWAPS-1LOAN-WHAT-IF-DEMO.json)** from this repo
2. Open **Claude Desktop**
3. Drag and drop the JSON file into the Claude chat
4. Paste this prompt:

```
Please run the DRAPS supply chain tariff GTAP-based sensitivity analysis
for commodities for supply chain finance simulation for this file.
Show me the market risk factors, SOFR rates, tariff changes across time,
risk-adjusted financing rates, original loan details, a key metrics table
with total cost of loan, and compare Scenario A / B / C with swap recommendations.
```

5. Claude runs the simulation and shows you charts, tables, and the winner analysis instantly.

>## OPTION A: 
to get response from local host use these json files: 
"<path-to-your-clone>\Swaps-for-Supply-Chain-Finance\SWAPS-interface\Frontend\DEMO-SWAPS\local\supplychain-tariff

## OPTION B:
if localhost doesnot work, check your hosted files in system 32 and also try 127.0.0.1
"<path-to-your-clone>\Swaps-for-Supply-Chain-Finance\SWAPS-interface\Frontend\DEMO-SWAPS\hosted\supplychain-tariff"

---

> **Plain English:** DRAPS is a tool that helps SME exporters hedge against sudden tariff changes (like US-India trade tariffs) using standardized financial contracts. You run a simulation, pick a hedging strategy, and see exactly what it costs — in real numbers, not guesswork.

---

## What Problem Does DRAPS Solve?

When tariffs spike mid-contract (e.g., 50% → 60% on India textiles), a business that locked in a loan rate at the start suddenly faces much higher costs. Banks and SMEs have no standard way to hedge this — until now.

DRAPS introduces a standardized contract structure using ACTUS (the same financial contract standard trusted by the FDIC and ECB) to make tariff risk **hedgeable, machine-readable, and accessible to SMEs**.

---

## How It Works — In Simple Terms

1. **You have a trade finance loan** (e.g. $1M, 2-year, India → US, textiles)
2. **A tariff shock hits** mid-loan (e.g. goes from 50% to 60%)
3. **DRAPS runs 3 scenarios** and tells you which strategy saves you money:

| Scenario | What it means | When to use |
|----------|--------------|-------------|
| **A — No Hedge** | Do nothing, absorb the cost | Baseline (worst case) |
| **B — Swap Now** | Buy protection immediately at 5% fixed | Best if shock is coming soon |
| **C — Swap Later** | Wait 3 months, pay 6% fixed | Risky — market charges you for waiting |

The simulation computes **exact cash flows** using ACTUS PAM (loan) + SWAPS contracts — no mocked data, real numbers.

---

## The Technical Ingredients

### GTAP Elasticity (σ) — the secret sauce
Each commodity has a different sensitivity to tariff shocks:

| Commodity | σ Score | Meaning |
|-----------|---------|---------|
| Textiles | 3.8 | Moderate — can find alternative suppliers |
| Pharmaceuticals | 2.4 | Low — hard to substitute |
| Electronics | 8.1 | Very high — easy to reroute |
| Crude Oil | 8.9 | Extremely substitutable |

Higher σ = bigger tariff transmission into your loan costs. DRAPS uses GTAP 11 data to calibrate this automatically.

### What Goes Into Each Simulation
- **SOFR** (base interest rate from US Federal Reserve)
- **Sovereign Stress** (e.g. India corridor: 50–70 bps)
- **Working Capital Stress** (30–50 bps based on trade corridor)
- **Tariff Rate** (loaded as an ACTUS Market Object — treated like an interest rate)
- **Armington Elasticity** from GTAP (determines how much the tariff transmits to your costs)

---

## How to Run a Simulation

### Step 1 — Prerequisites
Make sure you have the ACTUS engine running (Docker):
```bash
docker run -d -p 8082:8082 actus-risk-server
docker run -d -p 8083:8083 actus-simulation-server
```
If Docker is not running, the system automatically falls back to the hosted AWS server at `34.203.247.32`.

### Step 2 — Connect MCP Server to Claude Desktop
Add `generalRisk` MCP server to your Claude Desktop config. Once connected, 7 tools become available in Claude chat.

### Step 3 — Run a What-If Simulation
Open Claude Desktop, upload your simulation JSON file (e.g. `SWAPS-1LOAN-WHAT-IF-DEMO.json`), then paste this prompt:

```
Please run the DRAPS supply chain tariff GTAP-based sensitivity analysis 
for commodities for supply chain finance simulation for this file and 
produce the JSON response. Show me:
- Market risk factors across time
- Base interest rates (SOFR) across time
- Export/import country, commodity, and GTAP elasticity scores
- Tariff changes across time by commodity
- Risk-adjusted financing rates across time
- Original loan details
- Key metrics table including total cost of loan accumulated
- Compare and contrast the different hedging scenarios (A/B/C) 
  with total cost and swap recommendations
```

### Step 4 — Read the Output
Claude will generate:
- Interactive charts (tariff rates, SOFR, LTV, cumulative costs)
- A comparison table across Scenario A / B / C
- A **Winner Analysis** (Scenario D) telling you which hedge saves the most

---

## What's in This Repository

```
DRAPS/
├── Backend/                    ← MCP server source code (TypeScript)
│   ├── src/
│   │   ├── index.ts            ← Server entry point
│   │   ├── tools/              ← 7 simulation tools
│   │   └── utils/              ← ACTUS client, JSON parser, fallback logic
│   ├── collections/            ← 100+ pre-built simulation JSON files
│   │   ├── supplychain-tariff/ ← DRAPS core scenarios (India, China, Mexico)
│   │   ├── defi-liquidation-collateral/
│   │   ├── hybrid-treasury/
│   │   ├── stablecoin/
│   │   └── dynamic-discounting/
│   ├── config/                 ← Sample portfolios and test scenarios
│   ├── package.json
│   └── tsconfig.json
│
├── Frontend/                   ← UI (if applicable)
├── LegentvLEI/                 ← LEI verification module
│
├── SWAPS-1LOAN-WHAT-IF-DEMO.json   ← Demo simulation: India-US (tex/pha/oil)
├── actus_defi_loan_simulator.html  ← Standalone DeFi simulator UI
├── promts.md                       ← Saved prompt templates
└── README.md                       ← This file
```

---

## What to Keep in Your GitHub Repository ✅

**Keep these — they are YOUR work:**

| File/Folder | Why keep it |
|-------------|-------------|
| `Backend/src/` | Your MCP server code |
| `Backend/collections/supplychain-tariff/` | Your DRAPS simulation files |
| `SWAPS-1LOAN-WHAT-IF-DEMO.json` | Your demo simulation |
| `promts.md` | Your prompt templates |
| `README.md` | This file! |
| `package.json`, `tsconfig.json` | Needed to run the code |

**Be careful with these:**

| File/Folder | Note |
|-------------|------|
| `Backend/collections/` (other domains) | These belong to ChainAIM team — check with them |
| `actus_defi_loan_simulator.html` | DeFi project — separate from DRAPS |
| `LegentvLEI/` | Shared module — confirm ownership before pushing |

---

## Architecture

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────────┐
│                 │     │                      │     │   ACTUS Risk Engine  │
│  Claude Desktop │────▶│  generalRisk         │────▶│                     │
│  + Your JSON    │     │  MCP Server          │     │  :8082 Risk Factors  │
│  + Your Prompt  │◀────│  (7 tools)           │◀────│  :8083 Simulations   │
│                 │     │                      │     │                     │
└─────────────────┘     │  Parses JSON,        │     │  Docker (local) OR  │
        │               │  adds GTAP data,     │     │  AWS (fallback)     │
        ▼               │  runs simulation     │     └─────────────────────┘
  Charts, Tables,       └──────────────────────┘
  Scenario Comparison,
  Winner Analysis
```

---

## Simulation Domains Supported

| Domain | What it models |
|--------|---------------|
| **Supply Chain Tariff** ← DRAPS | Tariff risk hedging for trade finance loans |
| DeFi Liquidation | ETH collateral protection with BufferLTV |
| Hybrid Treasury | BTC/ETH treasury with cash pool management |
| Stablecoin | Reserve management, MiCA/GENIUS Act compliance |
| Dynamic Discounting | Early settlement, factoring, invoice finance |

---

## References & Data Sources

- [ACTUS Financial Research Foundation](https://www.actusfrf.org/) — Contract standard
- [GTAP 11 Database](https://www.gtap.agecon.purdue.edu/) — Armington elasticity values
- [Federal Reserve SOFR](https://www.newyorkfed.org/markets/reference-rates/sofr) — Base rate
- [PIIE Tariff Tracker](https://www.piie.com/research/trade) — Tariff data
- [WTO Tariff & Trade Data](https://ttd.wto.org/) — Bilateral tariff rates
- [Model Context Protocol](https://modelcontextprotocol.io/) — MCP standard (Anthropic)

---

## Status (as of March 2026)

**Completed:**
- Working PAM + SWAPS simulations via ACTUS
- GTAP-grounded tariff risk factor engine
- MCP server with 7 tools for NLP-driven simulation
- Multi-scenario what-if (A/B/C + Winner Analysis)
- India-US corridor: Textiles, Pharmaceuticals, Crude Oil

**Coming Next:**
- Live tariff feed integration (WTO, PIIE)
- Monte Carlo VaR across 1,000+ tariff paths
- Multi-corridor expansion (Japan, EU, Saudi Arabia)
- SME-facing production interface

---

