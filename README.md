# project2-stock-bot

## Project Overview
This project investigates whether significant declines in stock prices are followed by statistically identifiable periods of price recovery. The aim is to test a series of mean-reversion hypotheses across US equities and determine whether combinations of price action, company characteristics and broader market conditions can identify favourable trading opportunities.

The project will begin with historical validation and progressively develop into a stock-screening and paper-trading system. Automated execution will only be considered after the underlying strategy has been tested out-of-sample and evaluated for transaction costs, risk and robustness.

## Initial Research Question

Following a substantial decline in price, under what conditions is a US stock more likely to recover by a specified percentage within a specified period?

Candidate explanatory factors include:
- magnitude and duration of the preceding decline
- market capitalisation
- profitability and valuation
- proximity to 52-week or all-time lows
- broader market direction

The first stage of the project will test these relationships historically rather than assume that buying after a decline is inherently profitable.

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
