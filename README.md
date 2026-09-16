# Anupama Jain

**Building algorithmic trading infrastructure for Indian equity & derivatives markets.**

Most of my day is spent on systems where correctness is not negotiable – a bug in a strategy
engine is not a rendering glitch, it is a filled order. I work end to end: market-data
plumbing, strategy engines, backtesting, risk gates, and the dashboards traders actually
look at.

---

## 🛰 Orbis – investing platform driven by data

A production algo-trading and backtesting platform ([orbis.vriddhix.ai](https://orbis.vriddhix.ai)),
built and operated solo. Nine modules over one live market-data spine.

**What it does**

- **Strategy Studio** – visual condition-tree builder, 39 pre-built templates across technical,
  option-selling and price-action styles. No code required to ship a strategy.
- **AlgoHub** – automated strategies running as isolated OS processes, scanning 500 Nifty stocks
  every morning and managing positions end to end.
- **Option Buyer Battlefield** – weighted directional signal blending wall proximity, dealer
  gamma exposure, PCR trend, straddle expansion and IV percentile into one state machine.
- **Gamma Blast** – dealer-GEX / zero-gamma-flip read with threshold alerting.
- **Smart Money · Sector Rotation · News Desk · Straddle Premium · OI Tracker** – institutional
  flow, RRG rotation, real-time sentiment classification, and full-DTE straddle history.

**How it is built**

| Layer | Stack |
|---|---|
| Frontend | React 18 · TypeScript · Vite · Tailwind · shadcn/ui · Recharts |
| API / orchestration | Node.js proxy, process registry, WebSocket fan-out |
| Strategy engines | Python · Polars · DuckDB · VectorBT · `uv` |
| Data & state | Supabase (Postgres + RLS) · TimescaleDB · Redis · Parquet lake |
| Brokers | Upstox · Fyers · Dhan (broker-agnostic adapter layer) |
| AI | LangGraph agent, Groq → OpenAI → Gemini fallback chain |
| Infra | Vercel · AWS EC2 · GitHub Actions · PM2 · nginx |

**Engineering decisions I would call out**

- **Process-per-strategy isolation** – one strategy crashing can never take down another, or the
  platform.
- **One `BaseStrategy` ABC over a shared `ExecutionEngine`** – the same strategy code runs in
  backtest, paper and live. No separate live path to drift out of sync.
- **Paper-first by default** – every strategy simulates fills through a `PaperBook`; live
  execution is admin-gated behind a risk gate and an append-only order audit log.
- **Restart-proof state** – per-day counters and position books are derived from the reloaded
  book, not in-memory, so a respawn cannot silently reset a risk limit.

> Orbis is analysis and execution tooling, not advice. Not SEBI registered.
> The repository is private; happy to walk through architecture on a call.

---

## 🔬 Tathya – open-source pattern research

**[algo-backtesting-platform](https://github.com/AnupamaJain/algo-backtesting-platform)** –
three self-hosted tools in one repository: **Tathya**, a pattern-research
platform; a strategy backtester; and a live options-trading dashboard for
NIFTY/SENSEX.

Tathya – तथ्य, "that which is so" – is the part I would point at. It finds chart structure in NSE equities – volatility
contraction bases, breaks of market structure, fair value gaps – places each setup in market
and sector context, scores it, and then records what actually happened next. Including every
time it was wrong.

**What it measures**

| Engine | Finds |
|---|---|
| VCP | Contraction bases – prior trend, contractions, pivot, breakout |
| Market structure | Swings, BOS, CHoCH, order blocks, liquidity sweeps |
| Fair value gaps | Three-candle imbalances, tracked to mitigation |
| Market regime | Five weighted components with hysteresis, or an explicit refusal |
| Relative strength | Percentile rank across the universe, four blended horizons |
| Sector rotation | Equal-weight aggregation into leading / improving / weakening / lagging |

**Engineering decisions I would call out**

- **One implementation of every rule** – the live scanner, the backtester and the historical
  X-Ray call the same function objects. There is no second implementation to drift from, and a
  test walks the syntax tree to enforce it rather than trusting convention.
- **Nothing may see the future** – a value dated *t* depends only on bars dated *≤ t*, verified
  by truncating history, recomputing and demanding identical output. A look-ahead bug never
  crashes; it produces a backtest that looks excellent and means nothing.
- **It says what it does not know** – an unranked sector reads `null`, not `0`. A win rate over
  zero resolved trades is `null`, not `0%`. Absence is never quietly rendered as a bearish
  number, all the way through to the JSON.
- **Failures are kept** – failed breakouts stay in the ledger with their reason, and no view
  drops them by default. A hit rate computed over survivors is the single most flattering lie a
  research tool can tell.
- **Bias travels with the numbers** – a backtest that cannot resolve point-in-time index
  membership says so in its own results payload, not in documentation nobody reads.

**Stack** – Python · FastAPI · SQLAlchemy 2.0 · Alembic · pandas/numpy · SQLite → Postgres
/ TimescaleDB · 451 tests including no-look-ahead and engine-purity suites.

> Research and execution tooling, not advice. Not SEBI registered. The trading dashboard
> ships in dry-run mode; Tathya has no trading path at all.

---

## 🤖 Agentic AI & LLM engineering

Public repositories, mostly around putting LLMs behind guardrails rather than in front of them:

- **[orbis-quant-agents](https://github.com/AnupamaJain/orbis-quant-agents)** – LangGraph agents
  for market analysis, with validated tool contracts and provider fallback.
- **[llm-support-quality-gate](https://github.com/AnupamaJain/llm-support-quality-gate)** –
  quality gating for LLM-generated support responses.
- **[agenticworkflow-stlc](https://github.com/AnupamaJain/agenticworkflow-stlc)** – agentic
  workflows mapped onto the software testing lifecycle.
- **[agenticai-project-context](https://github.com/AnupamaJain/agenticai-project-context)** –
  project-context tooling for agentic coding sessions.

---

## 🧰 Working with

`Python` · `TypeScript` · `React` · `Node.js` · `Polars` · `DuckDB` · `Pandas` · `FastAPI`
`PostgreSQL` · `TimescaleDB` · `Redis` · `Supabase` · `Docker` · `AWS` · `LangGraph`

---

## 📬 Contact

- ✉️ [hello@vriddhix.ai](mailto:hello@vriddhix.ai)
- ▶️ [VriddhiX Wealth on YouTube](https://www.youtube.com/@vriddhixwealth) – market analysis &
  platform walkthroughs
- 💼 Available for freelance / contract engagements

<sub>Most of my commit activity is in private repositories, so the graph below is fuller than the
public repo list suggests.</sub>
