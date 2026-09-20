# Stock Bot V1

A research-to-execution quantitative trading project investigating short-term price behaviour after extreme single-day declines in US large-cap equities.

The project began with a simple question:

> **When a stock experiences an unusually large one-day decline, is what happens next meaningfully different from an ordinary trading day?**

Five historical experiments were used to test that question, moving from an event study to matched-control simulations, forward-return analysis, executable strategy backtesting and robustness testing.

The research identified a short-lived next-session rebound following ≥8% single-day declines. That finding was then translated into **Stock Bot V1**, a functioning paper-trading system connected to Alpaca that scans market data, detects qualifying signals, sizes positions, submits paper orders, tracks fills and positions, exits trades and records completed trades.

The project intentionally stops at paper trading. The historical results are evidence for further testing — not proof of a persistent live-trading edge.

---

## Project at a Glance

**Research period:** 2016–2025  
**Universe:** 10 US large-cap stocks  
**Extreme-drop definition:** ≥8% close-to-close decline  
**Extreme-drop events:** 60  
**Matched-control simulations:** 1,000  
**Final strategy:** Next-open → next-close  
**Historical mean net return per trade:** +1.24%  
**Historical median net return per trade:** +0.73%  
**Historical win rate:** 56.7%  
**Historical profit factor:** 2.14  
**Execution environment:** Alpaca paper trading  
**V1 status:** Complete

---

# Research

## Initial Research Question

Following a substantial decline in price, under what conditions is a US stock more likely to recover by a specified percentage within a specified period?

The project initially considered several possible explanatory factors, including:

- magnitude and duration of the preceding decline
- market capitalisation
- profitability and valuation
- proximity to 52-week or all-time lows
- broader market direction

Rather than assuming that buying after a decline is inherently profitable, the first research cycle isolated a simple price-action signal and tested it historically.

---

## Research Summary

| Experiment | Question | Main finding |
|---|---|---|
| **001 — Event Study** | Do stocks tend to recover after a ≥8% single-day decline? | 58.3% reached +5% within 10 trading days. |
| **002 — Matched Baseline** | Is that recovery rate unusual relative to ordinary trading days? | 58.3% recovery rate versus a 35.8% matched-control average. |
| **003 — Forward Returns** | What does the post-drop return path actually look like? | Strong Day-1 rebound, weakness over Days 3–5 and elevated volatility. |
| **004 — Strategy Backtest** | Can the effect survive realistic next-day entry timing? | A next-open → next-close strategy produced a +1.24% historical mean net return per trade after assumed costs. |
| **005 — Robustness** | Is the one-session result stable enough to justify paper trading? | The result remained positive across ticker exclusions, nearby thresholds, market regimes and higher transaction-cost assumptions. |

The evidence narrowed the original mean-reversion hypothesis considerably. The historical pattern was better characterised as a **short-term post-drop rebound** than a general multi-day recovery strategy.

---

# Experiment 001 — Large Single-Day Declines

### Hypothesis

US large-cap stocks that fall by at least 8% in one trading day exhibit a measurable tendency to subsequently recover.

**Event:** Daily close-to-close return ≤ −8%  
**Universe:** 10 US large-cap equities  
**Historical period:** 2016–2025  
**Outcome:** Whether the stock rises at least 5% from the post-drop closing price within the following 10 trading days

### Result

Across **60 qualifying events**, **58.3% reached the +5% recovery threshold within 10 trading days**.

This established the phenomenon of interest, but not a trading edge. The sample was small, event frequency varied substantially by stock, and the result still required comparison against ordinary trading days.

---

# Experiment 002 — Comparing Against Ordinary Trading Days

### Hypothesis

US large-cap stocks are more likely to gain 5% within 10 trading days after an ≥8% one-day decline than after an ordinary trading day.

To establish a baseline, the 60 extreme-drop observations were compared against **1,000 ticker-matched control samples**, each containing 60 ordinary trading-day observations.

### Result

- **Extreme-drop recovery rate:** 58.3%
- **Mean matched-control recovery rate:** 35.8%
- **Difference:** +22.5 percentage points
- **Median control recovery rate:** 35.0%
- **95% range of control simulations:** 25.0%–48.3%
- **Control simulations matching or exceeding the event result:** 0 / 1,000

Large-drop events therefore recovered substantially more often than the matched ordinary-day samples in this historical dataset.

![Extreme drops compared with ordinary trading days](data%20visualisations/01_extreme_drop_recovery.jpg)

---

# Experiment 003 — What Happens After the Drop?

A binary +5% recovery threshold does not describe the path a stock takes after an extreme decline. Forward returns were therefore measured after **1, 3, 5 and 10 trading days**.

### Result

Average post-drop returns were:

| Forward horizon | Mean return |
|---|---:|
| 1 day | +2.10% |
| 3 days | −0.96% |
| 5 days | −1.23% |
| 10 days | +2.65% |

The strongest immediate behaviour occurred on the **next trading day**. Returns then reversed over Days 3–5 before becoming positive again by Day 10.

The paths were also substantially more volatile than ordinary trading days. Extreme-drop events experienced both larger upside excursions and deeper downside excursions.

The result therefore did **not** resemble a smooth multi-day recovery.

![Forward returns following an extreme drop](data%20visualisations/02_forward_return_horizons.jpg)

---

# Experiment 004 — Turning the Finding Into a Tradable Rule

The previous experiments measured returns from the extreme-drop day's closing price. In practice, however, the ≥8% signal cannot be confirmed until that trading session has closed.

The backtest was therefore redesigned around information that would actually have been available at the time.

### Executable Rule

**Signal:** Stock closes ≥8% below its previous trading-day close  
**Entry:** Following trading session at the open  
**Exit:** Same trading session at the close

Alternative 3-, 5- and 10-session holding periods and a 5% take-profit rule were also tested.

### Result

The **one-session hold** produced the strongest historical combination of average return, win rate and profit factor.

| Strategy | Mean net return | Win rate | Profit factor |
|---|---:|---:|---:|
| **1-day hold** | **+1.24%** | **56.7%** | **2.14** |
| 3-day hold | −2.20% | 31.4% | 0.36 |
| 5-day hold | −2.25% | 28.6% | 0.49 |
| 10-day hold | +0.03% | 48.9% | 1.01 |
| 5% take-profit / 10-day max | +0.18% | 63.5% | 1.07 |

The 5% take-profit rule generated more winning trades, but its losing trades were sufficiently large to leave its average return substantially below the one-day strategy.

The historical hypothesis was therefore narrowed again:

> **The signal was not simply "buy a crash and wait for recovery." The strongest tested implementation was a short next-session rebound trade.**

![Strategy comparison](data%20visualisations/03_strategy_comparison.jpg)

---

# Experiment 005 — Robustness Testing

Before implementing the rule in a paper-trading system, the one-day strategy was subjected to several robustness checks.

Tests included:

- matched ordinary-day simulations
- bootstrap resampling
- alternative decline thresholds
- leave-one-stock-out analysis
- period and year sensitivity
- market-regime analysis
- transaction-cost sensitivity

### Matched Controls

The ≥8% strategy produced a **+1.24% historical mean net return**, compared with approximately **−0.05% across matched ordinary-day samples**.

None of the 1,000 matched-control simulations produced a mean return as high as the observed strategy result.

### Bootstrap Analysis

The bootstrap analysis produced a **95% interval of approximately +0.22% to +2.30%** around the observed historical mean return.

### Threshold Sensitivity

The mean return remained positive around the selected threshold:

| Decline threshold | Mean net return |
|---|---:|
| ≥6% | +0.75% |
| **≥8%** | **+1.24%** |
| ≥10% | +0.49% |
| ≥12% | +0.50% |

The ≥8% threshold was retained rather than further optimising against the historical sample.

### Stock Concentration

Each stock was removed from the universe in turn and the strategy was recalculated.

Mean net return remained positive in every leave-one-stock-out test, ranging from approximately **+0.81% to +1.48% per trade**.

Removing NVDA produced the largest deterioration, but the remaining strategy still generated a positive historical mean return.

![Leave-one-stock-out robustness test](data%20visualisations/03_strategy_robustness_check.jpg)

### Transaction Costs

The main backtest assumes **0.10% round-trip trading friction**. The strategy was additionally stress-tested under increasingly severe cost assumptions.

| Round-trip cost | Mean net return |
|---|---:|
| 0.00% | +1.34% |
| **0.10% — V1 assumption** | **+1.24%** |
| 0.25% | +1.09% |
| 0.50% | +0.84% |
| 1.00% | +0.34% |

The historical mean remained positive under every transaction-cost assumption tested, including a 1.00% round-trip cost — ten times the V1 assumption.

![Transaction-cost sensitivity](data%20visualisations/03_transaction_cost_sensitivity.jpg)

### Decision

These tests were considered sufficient to justify **out-of-sample paper trading**, while continuing to treat the strategy as experimental.

At this point, strategy development was deliberately frozen rather than continuing to optimise historical parameters.

---

# Stock Bot V1 — Paper-Trading Implementation

The research signal was translated into a functioning paper-trading workflow using the **Alpaca paper-trading environment**.

V1 is designed to test two things:

1. whether the historical signal continues to appear on unseen market data; and
2. whether the research logic can be translated into a functioning execution pipeline.

## V1 Strategy

**Universe:** Same 10-stock large-cap universe used in the research  
**Signal:** Close-to-close decline ≥8%  
**Entry:** Market buy during the following trading session  
**Exit:** Same-session market sell near the close  
**Maximum allocation:** 5% of account equity per qualifying signal  
**Environment:** Paper trading only

---

## V1 Workflow

```text
Alpaca Market Data
        │
        ▼
Download Daily Bars
        │
        ▼
Calculate Completed Close-to-Close Returns
        │
        ▼
Detect ≥8% Decline Signals
        │
        ▼
Check Existing Orders / Positions
        │
        ▼
Calculate Position Size
        │
        ▼
Submit Paper Buy Order
        │
        ▼
Track Order Fill
        │
        ▼
Track V1 Position
        │
        ▼
Final Session Exit Window
        │
        ▼
Submit Paper Sell Order
        │
        ▼
Confirm Exit Fill
        │
        ▼
Write Completed Trade to Log
        │
        ▼
Daily Session Summary
```

---

## What V1 Now Does

- retrieves current market data
- calculates completed close-to-close daily returns
- identifies ≥8% decline signals
- prevents duplicate entries within the running session
- calculates position sizes from paper-account equity
- submits market buy orders through Alpaca's paper environment
- retrieves broker order status and fill information
- identifies V1-owned open positions
- submits same-session exit orders
- confirms completed exits
- calculates realised trade return and P&L
- persists completed trades to a CSV trade log
- produces a daily execution summary

---

## Paper-Trading Validation

The execution pipeline was tested end-to-end in Alpaca's paper environment.

Test paper orders successfully progressed through:

**BUY submitted → BUY filled → position recognised → SELL submitted → SELL filled**

The broker activity and account history confirmed completed round trips, demonstrating that the V1 code can communicate with the brokerage environment and execute the intended order lifecycle.

> These validation trades test the **execution infrastructure**, not the historical strategy result. They should not be interpreted as out-of-sample evidence of profitability.

# V1 Architecture and Design Decisions

V1 was intentionally kept small enough that the complete research-to-execution pipeline remained understandable.

Several choices were deliberately frozen after the historical research:

- ≥8% decline threshold
- one-session holding period
- 5% maximum account allocation per signal
- same 10-stock universe
- no additional fundamental or technical filters
- no further historical optimisation

This reduces the temptation to continually tune the strategy against data it has already seen.

---

# V1 Limitations

V1 is a research and paper-trading prototype, not a production trading system.

Current limitations include:

- **Paper trading only.** No live capital is used.
- **Manual execution schedule.** The notebook does not independently wake itself at the required market times.
- **Session-state dependence.** Strategy ownership is currently maintained in memory during a notebook run rather than reconstructed from persistent state after a restart.
- **Simplified execution assumptions.** Historical next-open and next-close prices cannot perfectly reproduce real fills, spreads, slippage or market impact.
- **Limited universe.** Historical testing covers only 10 large-cap US stocks.
- **Limited event sample.** The main ≥8% event study contains 60 historical events.
- **IEX market-data feed.** V1 uses Alpaca's available IEX data feed rather than consolidated SIP data.
- **No live validation yet.** Successful paper-order execution demonstrates that the infrastructure works; it does not establish that the historical return pattern will persist out of sample.

These limitations are intentionally documented rather than hidden: the purpose of V1 is to establish a functioning experimental system from which genuine forward evidence can begin to accumulate.

---

# Technology

**Python** — research, analysis and execution logic  
**pandas / NumPy** — data processing and analysis  
**Matplotlib** — data visualisation  
**Alpaca API** — market data and paper-trading execution  
**Jupyter Notebook** — research and V1 implementation

---

# What V1 Establishes

Stock Bot V1 completes the first full research-to-execution cycle:

**Question → historical event study → statistical baseline → return analysis → executable backtest → robustness testing → frozen strategy → brokerage integration → paper execution**

The most important output of V1 is therefore not simply the historical +1.24% average return.

It is a functioning framework in which a hypothesis was progressively challenged, narrowed into an executable rule, stress-tested and then implemented in a paper brokerage environment without continuing to optimise against the historical sample.

---

# Next Steps — V2

V2 will focus primarily on **infrastructure and genuine forward testing**, rather than immediately searching for better historical parameters.

Potential improvements include:

- scheduled signal scanning and execution
- persistent strategy state across sessions
- robust order-status polling and reconciliation
- unique strategy/order identifiers
- automated execution and trade logging
- improved monitoring and error handling
- notifications and reporting
- longer-running out-of-sample paper-trading evaluation
- comparison between historical assumptions and observed paper fills

Only after sufficient forward evidence has accumulated should additional strategy changes or live-capital deployment be considered.

---

## Disclaimer

This project is an educational and research exercise. Historical and paper-trading results do not guarantee future performance and should not be interpreted as investment advice.
