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
- Initial historical period: 10 years
- Outcome: Whether the stock rises at least 5% from the post-drop closing price within the following 10 trading days
- Secondary variables: General market direction and company profitability

Primary metric: Percentage of qualifying events that achieve the 5% recovery threshold within 10 trading days.
