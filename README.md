[PREVIEW]

# NEXUS TERMINAL — Unified Intelligence Console for Multi-Exchange Futures Trading

![Version](https://img.shields.io/badge/version-2.4.0-2ea44f)
![License](https://img.shields.io/badge/license-MIT-blue)
![Python](https://img.shields.io/badge/python-3.11+-3776AB)
![Node](https://img.shields.io/badge/node-20.x-339933)
![Tests](https://img.shields.io/badge/tests-1,284%20passing-brightgreen)

## 🌌 Overview — Beyond the Single Exchange Lens

Every trader eventually hits the same wall: one exchange shows you a fragment, but the full picture lives across multiple venues. NEXUS TERMINAL is not another dashboard — it is a **unified intelligence console** that aggregates real-time futures data from multiple exchanges into a single, coherent command surface. Think of it as replacing a wall of mismatched monitors with a single holographic flight deck.

What began as a focused tool for one exchange's TensorAI signals has evolved into a **vendor-neutral analytics layer**. The system ingests raw order flow, funding rates, open interest shifts, and liquidation cascades from various exchanges, then applies a proprietary normalization engine to create apples-to-apples comparisons. The result: you finally see *relative strength* — not just absolute numbers.

This repository contains the complete source for the terminal backend (Python/FastAPI), the web frontend (React/TypeScript), and the optional desktop wrapper (Electron). Whether you operate a solo trading desk or coordinate a small team, NEXUS TERMINAL provides the interpretive layer that turns noisy market data into actionable hypotheses.

## 🧭 Why Another Market Dashboard?

The existing ecosystem is fractured. Exchange-native dashboards are biased toward their own liquidity books. Third-party aggregators often lag or simplify data to the point of uselessness. NEXUS TERMINAL occupies a middle ground: it respects the depth of native exchange data while providing a **cross-venue overlay** that reveals divergences, arbitrage windows, and sentiment shifts *before* they appear on any single platform.

The core philosophical difference: we treat every exchange as a *sensor*, not a source of truth. Truth emerges from the synthesis.

## 📡 Core Capabilities

### 🔄 Multi-Exchange Normalization Engine
- **Unified schema** for order books, trades, and positions across venues
- **Latency-aware clock synchronization** to correlate events across exchanges within 50ms windows
- **Currency-aware conversion** — handle USDT, USDC, BUSD, and native margin assets seamlessly
- **Error budget tracking** — the system self-reports when data feeds are strained

### 🧠 TensorAI Signal Interpretation Layer
- Parses and re-weights TensorAI confidence scores from underlying exchange models
- Applies **contextual dampening** — a signal during low liquidity is weighted differently than the same signal during high volume
- Generates a **Composite Conviction Score (CCS)** from 0 to 100, blending momentum, mean-reversion, and flow imbalance
- Exports signal history to CSV or Parquet for offline analysis

### 📊 Regime Detection & Risk Gauges
- **Volatility Regime Classifier** — identifies: *Contained*, *Expanding*, *Contracting*, or *Chaotic* states
- **Slippage Projector** — estimates realistic fills based on current book depth and historical spreads
- **Liquidation Heatmap** — visualizes where cascades are likely to trigger, based on leverage clusters

### 👥 Collaborative Workspace (Optional)
- Shared watchlists with role-based access
- **Signal comment threads** — attach rationale to any generated alert
- **Audit log** — full traceability of every decision trigger

## 🚀 Getting Started

### Prerequisites
- Python 3.11 or higher for the backend services
- Node.js 20+ for the frontend build
- A modern browser (Chrome 120+, Firefox 121+, Safari 17+)
- 4GB RAM minimum, 8GB recommended for multi-exchange parsing

### 🔧 Initial Configuration

**1. Environment Setup**
Create a configuration file from the provided template. Set your exchange API keys (read-only permissions recommended) and specify which data streams to activate. The system supports .env-style configuration or a JSON schema file for more complex setups.

**2. Backend Service Launch**
Start the normalization engine. The service will automatically detect available exchanges based on your configured keys and begin syncing metadata (instrument list, tick sizes, funding schedules).

**3. Frontend Connection**
Serve the web interface and point it to your backend instance. The UI will self-discover available data streams and populate the default dashboard layout.

[DOWNLOAD]

## 🎛️ Dashboard Interface Modules

### The Command Bar
The top-level control strip provides:
- **Venue Multi-Selector** — toggle exchanges on/off for live comparison
- **Global Timeframe** — sync chart intervals across all panels (1m to 1W)
- **CCS Threshold Slider** — filter signals by composite conviction score
- **Theme Toggle** — light, dark, and "amber on black" (vintage terminal) modes

### The Divergence Panel
This is the heart of NEXUS TERMINAL. For any selected symbol, it displays:
- **Funding Rate Delta** — the spread between perp funding rates across venues
- **Open Interest Momentum** — OI change rate, charted side-by-side for each exchange
- **Price Deviation Index** — how far each venue's mark price diverges from the volume-weighted median

### The Signal Stream
A live-updating feed of TensorAI-derived events. Each entry shows:
- Timestamp and originating exchange
- Raw signal type (e.g., "Breaker Block Reclaim")
- The **Composite Conviction Score** after normalization
- A one-line rationale describing the market micro-structure condition

### The Position Simulator
A sandbox module that lets you replay historical signals against the current book to estimate what *would* have happened. This is a tool for strategy validation, not live execution.

## 📈 Advanced Visualization Techniques

NEXUS TERMINAL employs a **candlestick-overlay** technique for multi-exchange charts. Instead of stacking charts vertically, we plot each exchange's OHLCV data as a semi-transparent overlay, with the volume-weighted aggregate as the opaque base layer. This reveals divergences as "ghosting" — where one venue's price action does not confirm the others.

For the open interest module, we use a **stream-flow diagram** that animates the flow of capital between exchanges for a given instrument. This helps identify where the "smart money" is positioning relative to the crowd.

## 🌍 Internationalization & Localization

The interface ships with production-quality translations for:
- English (default)
- 한국어 (Korean) — the original development language
- 日本語 (Japanese)
- 简体中文 (Simplified Chinese)
- Português (Brazilian)
- Türkçe (Turkish)

**Currency formatting** respects locale conventions, and **number parsing** handles both Western and East Asian digit grouping styles. Time zones are fully supported — dashboards can display exchange-local time, your local time, or a standardized UTC+0 reference.

The system also handles **right-to-left** script languages for future localization (Arabic, Hebrew) — the CSS layout supports RTL inversion.

## 🛡️ Reliability & Performance Engineering

### Fault Tolerance Philosophy
Each exchange connection runs in an isolated worker thread with a circuit-breaker pattern. If one venue's API becomes flaky, its data stream degrades gracefully (showing stale timestamp indicators) rather than crashing the entire terminal. The system will automatically attempt reconnection with exponential backoff.

### Performance Budgets
- **UI Interaction Latency**: Under 100ms for all control interactions
- **Chart Render Rate**: 60 FPS on standard hardware, even with 4 exchanges of tick data
- **Backend API Response**: p95 under 250ms for standard queries
- **Memory Footprint**: Base process under 450MB with all data streams active

### Data Persistence
Historical signals and normalized data are stored locally via SQLite by default, with an option to switch to PostgreSQL for larger deployments. The storage layer is designed for **append-heavy, read-light** workloads — typical for signal archives.

## 🔍 Security & Data Privacy

- All API keys are stored encrypted at rest using AES-256-GCM
- Key rotation is supported via the CLI or API
- Network traffic between frontend and backend can be wrapped in TLS
- The backend can run fully air-gapped — no telemetry is sent to any external service
- **Rate limit handling** is built-in for exchange APIs; the system will queue requests rather than trigger penalties

## 🤝 Extending the Terminal

### Plugin Architecture
NEXUS TERMINAL supports a Python-based plugin system for:
- Custom data transformers (e.g., VWAP re-calculations)
- Additional exchange adapters (if you have the API documentation)
- Alternative signal weighting algorithms
- Export connectors to other analysis tools

### Webhook Outbound
Configure POST webhooks to external systems (Telegram bots, Discord, or custom alerting infrastructure) when specific conditions are met — e.g., CCS crosses above 80 for a favorite symbol.

## 📚 Documentation & Learning Resources

- `/docs/architecture.md` — System design decisions and data flow diagrams
- `/docs/normalization-spec.md` — The precise math behind the cross-venue conversion
- `/docs/api-reference/` — OpenAPI 3.0 specification for the backend
- `/examples/` — Jupyter notebooks demonstrating signal backtesting workflows

## 🧪 Testing Suite

The repository includes a comprehensive test suite:
- **Unit tests**: 486 tests covering backend utilities and parsing logic
- **Integration tests**: 129 tests simulating multi-exchange data feeds
- **Frontend component tests**: 669 tests for React components and state management

Run the full suite with a single command from the project root. Continuous Integration builds are configured for GitHub Actions, running the suite on both Linux and Windows runners.

## 🗺️ Roadmap for 2026

- **Deliverable 1 (Q1)**: Real-time options chain aggregator
- **Deliverable 2 (Q2)**: Machine learning regime prediction module
- **Deliverable 3 (Q3)**: Native mobile companion app (iOS/Android)
- **Deliverable 4 (Q4)**: Collaborative signal sharing network between terminal instances

## 🤲 Community & Support

- **Issue Tracker**: Report bugs and suggest enhancements
- **Discussion Forum**: Share strategies and interpretation techniques
- **24/7 Support Channel**: For critical infrastructure questions, our community moderators and maintainers provide around-the-clock assistance via the project's support portal

We operate on a **contribution-first** model. The best way to get help is to help others — answer questions in the forum, review pull requests, or document your own signal interpretation workflow.

## ⚠️ Disclaimer

**This software is an analytical tool. It does not execute trades, manage funds, or provide financial advice.** The Composite Conviction Score (CCS) is a heuristic measure derived from historical patterns — it does not predict future market movements.

Trading futures and derivatives involves substantial risk of loss. Past performance of any signal or indicator does not guarantee future results. The developers, contributors, and maintainers of this project are not liable for any financial losses incurred through the use of this software. You are solely responsible for your own trading decisions.

Always conduct independent research and consider consulting with a qualified financial advisor before engaging in any trading activity.

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details. You are free to use, modify, and distribute this software for commercial or non-commercial purposes, provided the original copyright notice is retained.

---

**© 2026 NEXUS TERMINAL Project Contributors. All rights reserved.**

[DOWNLOAD]