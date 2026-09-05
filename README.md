# project2-stock-bot

## Project Overview
This project investigates whether significant declines in stock prices are followed by statistically identifiable trading opportunities across US equities.

The project began by testing mean-reversion hypotheses historically, using event studies, matched control simulations, forward-return analysis and strategy backtesting. The first research cycle identified evidence of a short-term rebound following large single-day declines and progressed the signal through robustness testing.

The project is now moving into the development of a **paper-trading system** that will generate signals, simulate trades and track performance on unseen market data.

Automated execution using live capital will only be considered after the strategy has been evaluated out-of-sample and the paper-trading infrastructure has been validated.

## Initial Research Question

Following a substantial decline in price, under what conditions is a US stock more likely to recover by a specified percentage within a specified period?

Candidate explanatory factors include:
- magnitude and duration of the preceding decline
- market capitalisation
- profitability and valuation
- proximity to 52-week or all-time lows
- broader market direction

The first stage of the project will test these relationships historically rather than assume that buying after a decline is inherently profitable.

## Research Summary

The first research cycle focused on a single price-action signal: large one-day declines in US large-cap stocks.

| Experiment | Question | Main Finding |
| --- | --- | --- |
| 001 | Do stocks tend to recover after a ≥8% single-day decline? | 58.3% reached +5% within 10 trading days. |
| 002 | Is that recovery rate unusual relative to ordinary trading days? | 58.3% recovery rate vs 35.8% matched-control average. |
| 003 | What does the full post-drop return path look like? | Strong Day-1 rebound, weakness over Days 3–5, and increased volatility in both directions. |
| 004 | Does the effect survive realistic next-day entry timing? | A next-open → next-close strategy returned 1.24% per trade after assumed costs; longer holds performed poorly. |
| 005 | Is the one-session strategy robust? | The effect outperformed matched controls and remained positive across nearby thresholds, ticker exclusions, market regimes and higher cost assumptions. |

The evidence therefore suggests that the original hypothesis is better characterised as a **short-term post-drop rebound effect** than a general multi-day mean-reversion strategy.

## Experiment 001 — Large Single-Day Declines

Hypothesis: US large-cap stocks that fall by at least 8% in one trading day exhibit a measurable tendency to subsequently recover.

- Event: Daily close-to-close return ≤ −8%
- Universe: Large-cap US equities
- Initial historical period: 01-Jan-2016–01-Jan-2026
- Outcome: Whether the stock rises at least 5% from the post-drop closing price within the following 10 trading days
- Secondary variables: General market direction and company profitability

Primary metric: Percentage of qualifying events that achieve the 5% recovery threshold within 10 trading days.

**Results**: A majority of qualifying events reached the recovery threshold (58.3%), but the result cannot yet be interpreted as evidence of a trading edge. The sample is small (10 companies), event frequency varies substantially by stock (range 1-19), and no baseline comparison or trading-cost analysis has yet been performed. 

## Experiment 002 — Comparing Against the Baseline

Hypothesis: US large-cap stocks are more likely to gain 5% within 10 trading days after an ≥8% one-day drop than they are after an ordinary trading day.

- Event: Daily close-to-close return ≤ −8%
- Control: Ordinary trading days with returns > −8%
- Universe: Same 10 large-cap US equities used in Experiment 001
- Historical period: 01-Jan-2016–01-Jan-2026
- Outcome: Whether the stock gains at least 5% from the starting close within the following 10 trading days
- Control design: 1,000 randomly sampled control groups, matched to the event sample by ticker

Primary metric: Difference between the event recovery hit rate and the average matched-control hit rate.

**Results**: Large-drop events produced a 58.3% recovery hit rate, compared with a mean control hit rate of 35.8%, an uplift of 22.5 percentage points. The median control hit rate was 35.0%, and 95% of simulated control samples fell between 25.0% and 48.3%. None of the 1,000 control simulations matched or exceeded the observed 58.3% event hit rate.

These results provide preliminary evidence that large single-day declines are associated with a higher probability of a subsequent 5% recovery than ordinary trading days in this sample. The next experiment will examine the full distribution of forward returns and downside outcomes rather than relying only on a binary recovery threshold.

## Experiment 003 — Forward Return Analysis

**Question:** What happens to returns after a ≥8% daily decline beyond simply reaching a 5% recovery threshold?

Forward returns were measured after 1, 3, 5 and 10 trading days and compared with ticker-matched ordinary trading days.

**Results:** Large-drop events produced a strong **2.10% average one-day return**, compared with 0.14% for matched controls. However, average returns fell to **-0.96% after three days** and **-1.23% after five days**, before recovering to **2.65% after ten days**.

The ten-day paths also showed substantially greater volatility. Large-drop events reached an average maximum return of **7.91%**, compared with 4.60% for ordinary days, but also an average minimum return of **-6.36%**, compared with -3.03%.

The results suggest that large declines are followed by an **immediate rebound and elevated volatility**, rather than a smooth multi-day recovery.

## Experiment 004 — Realistic Strategy Backtest

**Question:** Can the post-drop rebound still be captured if the strategy waits until the large-drop day has closed before entering?

Trades were entered at the **next trading day's open** and several exit rules were tested, including 1-, 3-, 5- and 10-session holds and a 5% take-profit rule.

**Results:** The strongest strategy was a **one-session hold**, entering at the next open and exiting at the same day's close.

- Mean net return: **1.24%**
- Median net return: **0.73%**
- Win rate: **56.7%**
- Profit factor: **2.14**

The three- and five-session strategies produced average losses of approximately **-2.2%**, while the ten-session strategy was approximately break-even.

The 5% take-profit strategy achieved a high 63.5% win rate but only a **0.18% mean return**, as unsuccessful trades generated substantially larger losses.

The results narrowed the strategy hypothesis to a **short-lived next-session rebound** rather than a longer-term recovery trade.

## Experiment 005 — One-Session Strategy Robustness

**Question:** Is the one-session rebound sufficiently stable to justify implementation in a paper-trading system?

The strategy was tested against matched ordinary trading days, alternative decline thresholds, ticker concentration, year and market-period effects, broader market regime and higher transaction-cost assumptions.

**Results:** The ≥8% strategy produced a **1.24% mean net return**, compared with approximately **-0.05%** across matched ordinary trading days. None of 1,000 matched control simulations achieved a mean return as high as the observed strategy.

A bootstrap analysis produced a 95% interval of **0.22% to 2.30%** around the observed mean return.

The effect also remained positive across nearby signal thresholds:

- ≥6% decline: **0.75%**
- ≥8% decline: **1.24%**
- ≥10% decline: **0.49%**
- ≥12% decline: **0.50%**

Removing NVDA reduced the mean return to **0.81%**, indicating that the result was not dependent on the stock that contributed the most signals.

Performance was materially stronger during the 2020 market disruption, although the strategy remained positive when 2020 was excluded. Returns were also positive both above and below the SPY 200-day moving average and remained positive under transaction-cost assumptions of up to 1%.

These results were considered sufficient to progress the strategy into **V1 paper trading**, while continuing to treat it as an experimental signal rather than a validated live-trading strategy.

## V1 — Paper-Trading Bot

Following the initial research cycle, the project is moving from historical analysis into a persistent paper-trading system.

### Strategy

The first implementation uses the strongest signal identified during Experiments 001–005:

> **Signal:** A stock closes at least 8% below its previous trading-day close.  
> **Entry:** Buy at the following trading day's open.  
> **Exit:** Sell at the same trading day's close.

The objective of V1 is not to optimise the strategy further, but to test whether the historically observed behaviour persists on unseen data while developing the infrastructure required for a functioning trading system.

### Planned V1 Workflow

1. Download the latest market data.
2. Calculate daily returns across the stock universe.
3. Identify stocks meeting the ≥8% decline threshold.
4. Generate paper orders for the following trading session.
5. Allocate equal simulated capital across qualifying positions.
6. Record entry prices at the next open.
7. Exit positions at the same day's close.
8. Update cash, portfolio value and realised P&L.
9. Maintain a permanent signal and trade history.

### Initial V1 Components

- [ ] Market data pipeline
- [ ] Signal scanner
- [ ] Paper portfolio
- [ ] Position sizing
- [ ] Entry logic
- [ ] Exit logic
- [ ] Trade history
- [ ] Performance tracking
- [ ] Daily reporting

### Initial Constraints

V1 will remain intentionally simple.

- No live capital
- No automated brokerage execution
- Equal-value positions
- One-session holding period
- Fixed ≥8% signal threshold
- No fundamental or technical filters
- No optimisation based on new paper-trading results

Additional strategies, including 52-week lows, prolonged declines, fundamentals and technical indicators, will be developed as separate research modules after the V1 system is operational.
