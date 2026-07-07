# Open Financial Terminal — macOS

A native macOS build of **Open Financial Terminal**: a Bloomberg-style, dockable-widget financial
workspace — live charts, an options desk, a factor screener, backtesting, portfolio & risk, paper
trading, SEC filings, macro/rates, and a fleet of LLM research agents — running as a real Mac app,
**no browser and no login**.

<p align="center">
  <img src="docs/screenshots/hero.png" alt="Open Financial Terminal on macOS" width="900">
</p>

<p align="center">
  <img alt="platform" src="https://img.shields.io/badge/macOS-12%2B-black?logo=apple">
  <img alt="arch" src="https://img.shields.io/badge/Apple%20Silicon-arm64-0a7">
  <img alt="modules" src="https://img.shields.io/badge/modules-34-blue">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-blue">
  <a href="https://github.com/Yimunan/open-financial-terminal-mac-os/releases/latest">
    <img alt="release" src="https://img.shields.io/github/v/release/Yimunan/open-financial-terminal-mac-os?display_name=tag"></a>
</p>

---

## Contents

- [Install](#-install)
- [The workspace](#the-workspace)
- [Modules](#modules) — a screenshot + description of all **34** widgets
- [Data & configuration](#data--configuration)
- [Build from source](#build-from-source)
- [Notes & caveats](#notes)
- [Credits & license](#credits--license)

---

## ⬇️ Install

1. Download **`OpenFinancialTerminal.dmg`** from the
   [latest release](https://github.com/Yimunan/open-financial-terminal-mac-os/releases/latest).
2. Open it and **drag "Open Financial Terminal" onto the Applications folder**.
3. First launch — because the app isn't Apple-notarized, macOS will warn. **Right-click the app →
   Open → Open** (once). If it's still blocked on macOS 15+: **System Settings → Privacy & Security →
   Open Anyway**. Equivalent one-liner:
   ```bash
   xattr -cr "/Applications/Open Financial Terminal.app" && open "/Applications/Open Financial Terminal.app"
   ```

> First launch takes ~20–30s (it unpacks a self-contained runtime once), then it's fast.
> Full details in [INSTALL-macOS.md](INSTALL-macOS.md).

**Requirements:** macOS 12+ on **Apple Silicon (M1/M2/M3/M4…)**. This build is `arm64`-only.

---

## The workspace

The terminal is a single window tiled with **draggable, resizable, tabbed widgets** — a Dockview
"bento" grid you rearrange freely, split into panes, tab together, maximize, or pop out into their
own native window (⧉).

A few conventions run through every module:

- **⌘K command bar** — open any module, jump to a symbol, or apply a workspace template.
- **Link channels** — each widget carries a colored dot (🔴 red / 🔵 blue / 🟢 green, or none). Widgets
  on the same channel **share the active symbol**, so picking `NVDA` in a Watchlist retargets the
  linked Chart, Quote, Order Book, News and Options in one click. Different channels track different
  symbols side-by-side.
- **Freshness badges** — every value is tagged **LIVE / DELAYED / EOD** so you always know how fresh
  a number is.
- **Workspace templates** — ready-made desks (Equities Day-Trading, Crypto, Research & Due Diligence,
  Quant Research, Portfolio & Risk, Macro & Markets, Algo/Execution) you can apply and then save your
  own layouts.
- **Cross-module "Send to…"** — hand a result off between modules (a screen → a watchlist, a backtest
  → paper trading, a factor → the backtester).

---

## Modules

All 34 modules at a glance — jump to any section below.

| Market & charts | Options | News & filings | Quant & backtesting | Portfolio & risk | Execution | Macro / FICC | Research & AI |
|---|---|---|---|---|---|---|---|
| [Watchlist](#watchlist) · [Market Board](#market-board) · [Quote](#quote) · [Profile](#profile) · [Chart](#chart) · [Chart Studio](#chart-studio) · [Order Book](#order-book) · [Time & Sales](#time--sales) | [Option Chain](#option-chain) · [Options Surface](#options-surface) | [News](#news) · [Topic News](#topic-news) · [Public Filings](#public-filings) · [New Listings](#new-listings) | [Screener](#screener) · [Factors](#factors) · [Factor Performance](#factor-performance) · [Backtest](#backtest) · [Strategies](#strategies) · [Models](#models) · [Sandbox](#sandbox) | [Portfolio](#portfolio) · [Portfolio Builder](#portfolio-builder) · [Risk Attribution](#risk-attribution) | [Paper Trading](#paper-trading) · [Algo Trading](#algo-trading) · [Market Making](#market-making) | [Macro](#macro) · [FICC](#ficc) | [Assistant](#assistant) · [Agent Workflow](#agent-workflow) · [Research Loop](#research-loop) · [Committees](#committees) · [Map](#map) |

> Screenshots below are captured from the running app with live data — real EDGAR filings, FRED
> macro series, market bars, a sample holdings book and paper positions, and an LLM-built chart.
> Two exceptions render their **empty state**: Time & Sales (the equity tape needs a live feed —
> add Alpaca keys) and Risk Attribution (needs positions with factor coverage).

### Market data & charts

#### Watchlist
A live, linkable list of symbols with a 30-day sparkline, last price and % change. Equities poll
delayed/EOD quotes; crypto rows ride the live ticker stream; an optional NBBO bid/ask sub-line sits
under the price. Add tickers inline and link the list to a channel to drive the rest of your desk.

<img src="docs/screenshots/widgets/watchlist.png" alt="Watchlist" width="420">

#### Market Board
A cross-asset quote board with tabs for **Equities, Commodities, Bonds & Rates, FX and Crypto**, plus
a cross-asset **Correlation** view — indices and benchmarks with sparklines and change at a glance.

<img src="docs/screenshots/widgets/market_board.png" alt="Market Board" width="820">

#### Quote
A focused single-symbol quote: large last price and change, sparkline, bid/ask, spread, day high/low
and volume, with a freshness badge. Enter Alpaca keys for live NBBO bid/ask.

<img src="docs/screenshots/widgets/quote.png" alt="Quote" width="420">

#### Profile
Everything about one name on three tabs — **Metrics / Chart / Fundamentals**: valuation (market cap,
P/E, P/B), profitability & quality (ROE, margins), growth, risk/return (volatility, Sharpe, Sortino,
max drawdown, Calmar, beta), trailing returns and the 52-week range.

<img src="docs/screenshots/widgets/metrics.png" alt="Profile" width="820">

#### Chart
A price chart with **candles / Heikin-Ashi / area**, indicator overlays (SMA, EMA, RSI, MACD,
Bollinger) and 1m→1d timeframes, drawn on a from-scratch canvas engine with a volume histogram below.

<img src="docs/screenshots/widgets/chart.png" alt="Chart" width="820">

#### Chart Studio
A **chat-driven charting canvas**: describe the chart you want in plain English — *"AAPL daily with a
50-day SMA and RSI"*, *"compare AAPL, MSFT, NVDA over 1 year"* — and it builds it, keeping a history of
past charts.

<img src="docs/screenshots/widgets/chart_studio.png" alt="Chart Studio" width="820">

#### Order Book
A live **L2 depth ladder** for any asset class, drawn as a depth heatmap (row width = cumulative size,
opacity ∝ level size; liquidity walls highlighted). Ships with a synthetic **sim** depth source and is
pluggable to real venues (IBKR / Databento / dxFeed).

<img src="docs/screenshots/widgets/orderbook.png" alt="Order Book" width="420">

#### Time & Sales
The live **tape** — streaming trade prints (price, size, aggressor side) for any asset class. Equities
stream from a real feed (Alpaca); rates/FX/commodities use a built-in simulated tape.

<img src="docs/screenshots/widgets/timesales.png" alt="Time & Sales" width="420">

### Options

#### Option Chain
A full equity **options chain** (calls | strike | puts) with bid/ask, implied vol and greeks per
expiry. Select contracts to build a single-leg ticket and paper-trade it.

<img src="docs/screenshots/widgets/options_chain.png" alt="Option Chain" width="820">

#### Options Surface
The implied-volatility structure for an underlying: the per-expiry **IV smile** (calls vs puts across
strikes) and an expiry×strike **IV surface heatmap**.

<img src="docs/screenshots/widgets/options_surface.png" alt="Options Surface" width="820">

### News & filings

#### News
Per-symbol headlines for the linked ticker, each **LLM-scored for sentiment** and composite-ranked,
aggregated across sources with timestamps.

<img src="docs/screenshots/widgets/news.png" alt="News" width="480">

#### Topic News
Symbol-agnostic **topic feeds** — built-in Market/Macro streams or your own interest topics — ranked
and searchable, each topic its own draggable tab.

<img src="docs/screenshots/widgets/topicnews.png" alt="Topic News" width="480">

#### Public Filings
**SEC/EDGAR** filings for a company (Financials, Events, Insider, Ownership, Governance, Offerings)
with quick access to the source documents, plus Insider and Institutional views.

<img src="docs/screenshots/widgets/filings.png" alt="Public Filings" width="820">

#### New Listings
Recent **exchange listings and IPOs** (8-A12B family / 424B4) over a chosen lookback, filterable by
form type.

<img src="docs/screenshots/widgets/listings.png" alt="New Listings" width="820">

### Screening, factors & backtesting

#### Screener
A factor and **natural-language screener**: pick a universe + factor and Run, or ask in plain English
— *"defensive low-vol names in the Dow"* — and a local LLM maps your words onto the qhfi factor
catalog.

<img src="docs/screenshots/widgets/screener.png" alt="Screener" width="820">

#### Factors
The **factor library** — built-in factors (momentum, volatility, reversal, value E/P & B/P, quality
ROE & gross-margin, the Alpha101 set, composites) plus the qhfi engine factors — to browse, summarize,
or open in the Sandbox.

<img src="docs/screenshots/widgets/factors.png" alt="Factors" width="820">

#### Factor Performance
An agent-driven single-factor **drill-down across three diagnostic layers** (Returns / Risk / Health):
rank factors, inspect IC, quantile spreads, decay and turnover, and save a monitor.

<img src="docs/screenshots/widgets/factor_monitor.png" alt="Factor Performance" width="820">

#### Backtest
A cross-sectional **factor backtester**, a single-instrument **Lab** with parameter sweeps, and a
**Market-Making** mode — with transaction costs, PSR / Deflated Sharpe and an equity curve. Chat-driven
with ready-made idea templates.

<img src="docs/screenshots/widgets/backtest.png" alt="Backtest" width="820">

#### Strategies
A library of strategies: built-in single-symbol Lab templates (SMA/EMA/RSI/MACD crossover, Bollinger,
Donchian) and qhfi engine portfolio strategies (MDP, model, momentum). Test one on the linked symbol
or backtest it over a universe.

<img src="docs/screenshots/widgets/strategies.png" alt="Strategies" width="820">

#### Models
The **model repository**: register and manage trained models that bundle a factor + strategy +
universe + params for reuse across research and execution.

<img src="docs/screenshots/widgets/models.png" alt="Models" width="820">

#### Sandbox
An **AST-restricted Python sandbox** for authoring custom factors/strategies: write a factor formula,
run it across a universe, and save it to your libraries — no imports, no I/O.

<img src="docs/screenshots/widgets/sandbox.png" alt="Sandbox" width="820">

### Portfolio & risk

#### Portfolio
A holdings book with tabs for **Holdings, Composition, Risk and Attribution**: positions, cost, P&L,
and per-name composition.

<img src="docs/screenshots/widgets/portfolio.png" alt="Portfolio" width="820">

#### Portfolio Builder
Construct and save a portfolio as a **weight + allocation list** (symbol → target weight) that you can
normalize (gross 100%, dollar-neutral for long/short) and value into share counts against a capital
base.

<img src="docs/screenshots/widgets/portfolios.png" alt="Portfolio Builder" width="820">

#### Risk Attribution
Portfolio-level **Barra factor + position risk decomposition** for the whole book, with Risk /
Realized / Brinson views and a Holdings/Paper source toggle.

<img src="docs/screenshots/widgets/risk_attribution.png" alt="Risk Attribution" width="820">

### Execution

#### Paper Trading
A paper-trading **blotter** (local simulator or **Alpaca paper**): order ticket, cash, unrealized /
realized P&L, positions, and an equity-curve sparkline.

<img src="docs/screenshots/widgets/paper.png" alt="Paper Trading" width="820">

#### Algo Trading
Scheduled/templated **algos** seeded from the linked symbol (e.g. an AAPL SMA-cross, a Dow30 momentum
long-only), each arm-able and runnable on a cadence for automated paper execution.

<img src="docs/screenshots/widgets/algo_trading.png" alt="Algo Trading" width="820">

#### Market Making
Compare **market-making quoting strategies** over real bars + synthetic depth, tuning book spread,
quote half-spread and inventory skew / limits.

<img src="docs/screenshots/widgets/market_making.png" alt="Market Making" width="820">

### Macro & FICC

#### Macro
A macroeconomic dashboard over the qhfi lake (**FRED + World Bank + Treasury curve**): a US indicators
grid, the Treasury yield curve, a series explorer over the full catalog, and a World Bank
cross-country panel.

<img src="docs/screenshots/widgets/macro.png" alt="Macro" width="820">

#### FICC
A unified **fixed-income / currencies / commodities** board: the Treasury yield curve and CME Treasury
futures complex (ZQ/ZT/ZF/ZN/ZB/UB), G10 spot FX, and the commodity futures complex
(metals/energy/agriculture), plus a cross-asset correlation tab. EOD daily bars.

<img src="docs/screenshots/widgets/ficc.png" alt="FICC" width="820">

### Research & AI

#### Assistant
A streaming, **symbol-aware LLM assistant** grounded in the terminal's live data via read-only tools
(quote, fundamentals, news, compare, screen, performance). It can also **open and configure modules**
for you.

<img src="docs/screenshots/widgets/assistant.png" alt="Assistant" width="480">

#### Agent Workflow
A **visual node-graph** you compile to a LangGraph agent: wire Data → Strategy → Portfolio →
Backtest/Execution nodes (and Quote, News, Factor-screen, Research Loop, Committee, LLM, Python steps).
With **Reveal** on, each node streams its output into the matching module.

<img src="docs/screenshots/widgets/agent.png" alt="Agent Workflow" width="820">

#### Research Loop
An autonomous **design → generate → evaluate → reflect** loop over the qhfi engine: give it a goal and
it designs a factor experiment, backtests it, grades it against the promotion scorecard, and iterates
up to 5 times — pinning the best.

<img src="docs/screenshots/widgets/research_loop.png" alt="Research Loop" width="820">

#### Committees
An **investment-committee simulation**: LLM agents (Bull/Growth, Bear/Risk, Macro Strategist, Chair)
whose assessments feed one another along a directed relationship graph toward a synthesized decision.
Runs on the external crew service or falls back to a local-LLM committee.

<img src="docs/screenshots/widgets/committee.png" alt="Committees" width="820">

#### Map
An SVG **module-wiring map** of the terminal: **Catalog** mode shows the static architecture (every
widget type, channel groups, send routes and the data layer); **Live** mode shows your open workspace.
Filter by edge type (Links / Sends / Data).

<img src="docs/screenshots/widgets/map.png" alt="Map" width="820">

---

## Data & configuration

- **Live public data out of the box** — equities via Yahoo Finance, crypto via ccxt (Kraken), SEC
  filings via EDGAR, Treasury curve via public rate series. No keys required for these.
- **Live equities & the tape (optional)** — add **Alpaca** paper keys under **⌘K → Settings → Market
  Data** for live equity quotes, NBBO bid/ask, the streaming Time & Sales tape, and Alpaca paper
  trading. Order-book depth ships with a built-in simulator and is pluggable to IBKR / Databento /
  dxFeed.
- **AI assistant / NL screener / agents** — open **⌘K → Settings → Model & provider** and point the
  terminal at any OpenAI-compatible API. Two equally supported flavors (the chip shows which one is
  active):
  - **Local API** — a server on your own machine, no key needed. Example (Ollama): base URL
    `http://localhost:11434/v1`, model `gemma4:e4b-it-qat` (or any model you've pulled).
  - **Online API** — a hosted provider, API key required. Example (DeepSeek): base URL
    `https://api.deepseek.com/v1`, model `deepseek-v4-flash`. **No key ships in the app.**
- App state (cache, settings, encrypted keys) lives under `~/OpenFinancialTerminal/`. Each install
  generates its own encryption key on first run.

---

## Build from source

The `.app`/DMG are produced by [`build/build-macos.sh`](build/build-macos.sh) — a PyInstaller freeze
of the FastAPI backend + qhfi engine + the built SPA, wrapped in a `.app` with a PyWebView window. See
[build/README.md](build/README.md) — **including the important qhfi note below.**

```bash
# Apple Silicon, macOS 12+, with `uv` and Node 18+ installed
bash build/build-macos.sh ~/oft-build
# → ~/oft-build/OpenFinancialTerminal.dmg
```

### Why a separate macOS repo (the qhfi gitignore fix)

The upstream terminal imports the `qhfi.data` and `qhfi.models` packages, but the public
[quant-hedge-fund-incubator](https://github.com/Yimunan/quant-hedge-fund-incubator) repo is
**missing** them — so a plain clone won't even import. Root cause: its `.gitignore` lists

```
data/
models/
```

for the regenerable data lake, and those patterns **also match the source packages**
`src/qhfi/data/` and `src/qhfi/models/`, so git silently dropped them on push
(`git check-ignore src/qhfi/data/base.py` → `data/`). The one-line fix is to anchor the rules to the
repo root (`/data/`, `/models/`) and re-commit the packages. This macOS build ships with a working
data/model layer already included, so the DMG runs regardless.

---

## Notes

- **Apple Silicon only** — no Intel/`x86_64` build.
- **Not notarized** — expect the Gatekeeper prompt on first open (see [Install](#-install)).
  Notarization needs an Apple Developer account.
- The `qhfi` data/model layer in this build is an independent reconstruction of the packages omitted
  from the public release; some advanced quant behaviors may differ from the original.
- Macro (FRED) cards and EDGAR filings require reaching `fred.stlouisfed.org` / `www.sec.gov`; where
  those hosts are blocked they stay empty (the Treasury curve falls back to Yahoo tickers).

## Credits & license

Open Financial Terminal and the qhfi engine are **MIT-licensed** by their authors
([open-financial-terminal](https://github.com/Yimunan/open-financial-terminal),
[quant-hedge-fund-incubator](https://github.com/Yimunan/quant-hedge-fund-incubator)). This repo adds
the macOS packaging + build tooling under the same license — see [LICENSE](LICENSE).

*Not investment advice. Data is provided by third parties for informational use only.*
