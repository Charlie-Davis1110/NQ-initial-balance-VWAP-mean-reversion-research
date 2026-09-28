# NQ Initial Balance → VWAP

A quantitative research and backtesting project investigating a mean-reversion strategy on NQ futures based on the Initial Balance (IB) and Volume Weighted Average Price (VWAP).

## Project Overview

The project uses historical NQ futures data from periods of relatively weak trend strength. Research periods were manually selected from the NQ daily chart where the daily ADX was below 25, indicating a more consolidating market environment.

The project follows a two-stage process:

1. **Research** – analyse IB High and IB Low → VWAP setups, measure maximum adverse excursion (MAE), and test different stop-loss values.
2. **Backtesting** – test the selected strategy under two different position-management approaches.

The aim is to investigate whether an Initial Balance break and reclaim can provide a systematic mean-reversion setup towards VWAP during more consolidating market conditions.

## Research Findings

The initial research compared two setups:

* **IB High → VWAP** – short mean-reversion setup
* **IB Low → VWAP** – long mean-reversion setup

The IB High → VWAP setup produced negative results across the stop-loss values tested and was therefore excluded from the backtest.

The IB Low → VWAP setup produced positive results across the stop-loss values tested. A 20-point stop-loss was selected based on the MAE analysis and the trade-off between allowing room for adverse movement and limiting losses.

## Backtesting Models

Two versions of the IB Low → VWAP strategy are tested.

### 1. Non-Conservative / High-Exposure Model

This model takes every valid setup, even when other positions are already open. As a result, multiple positions can be open simultaneously, creating substantially greater capital and risk requirements. The stop loss used was 20 points below the entry price.

This model is intended to investigate the performance of the strategy when capital and position capacity allow multiple simultaneous trades.

### 2. Conservative Model

This model allows only one active position at a time. A new setup can only be traded once the previous position has been closed, either at VWAP or at the 20 point stop loss. The model also moves the stop-loss to breakeven price once the price action has moved 20 points in the favorable direction, this helps to preserve capital.

This reduces simultaneous exposure and provides a more capital-constrained version of the strategy.

## Strategy Rules

Both backtest models use the same core strategy:

* **Market:** NQ futures
* **Initial Balance:** 9:30–10:30 New York time
* **Setup:** Price breaks below the IB Low and subsequently reclaims it
* **Entry:** IB Low reclaim
* **Target:** VWAP
* **Stop:** 20 points
* **Direction:** Long mean reversion

The difference between the two models is the management of simultaneous positions and the moving stop-loss to breakeven on the conservative model.

## Performance Analysis

The backtests evaluate:

* Total P&L
* Win rate
* Expectancy
* Profit factor
* Maximum drawdown
* Sharpe ratio
* Number of trades
* Simultaneous positions
* Transaction costs and slippage

## Results

The research showed that the IB Low → VWAP setup gave positive results across the stop-loss values tested, while the IB High → VWAP setup was consistently negative so was not used in backtest. A 20-point stop was used for the backtests based on the MAE results.

| Metric                         | Non-Conservative | Conservative |
| ------------------------------ | ---------------: | -----------: |
| Trading days                   |               79 |           77 |
| Trades                         |            9,364 |          131 |
| Trades per day                 |           118.53 |         1.70 |
| Win rate                       |           41.25% |       27.48% |
| Expectancy                     |         1.94 pts |     1.50 pts |
| Profit factor                  |             1.17 |         1.14 |
| Daily Sharpe                   |             1.33 |         0.96 |
| Net P&L                        |   +18,212.75 pts |  +196.50 pts |
| Net P&L                        |        +$364,255 |      +$3,930 |
| Maximum drawdown               |   -18,415.50 pts |  -380.00 pts |
| Maximum drawdown               |        -$368,310 |      -$7,600 |
| Maximum simultaneous positions |              185 |            1 |
| Average positions open         |            24.05 |         0.48 |

Transaction costs were included using 1 point of round-trip slippage and $5 round-trip commission, giving a total cost of 1.25 points per trade.

The non-conservative model produced much higher P&L, but also had much higher exposure and drawdown. The conservative model had lower returns but didn't require such a large account drawdown.


## Repository Structure

```text
NQ-IB-VWAP/
├── README.md
├── research/
│   └── IB_VWAP_Research.ipynb
└── backtests/
    ├── Non_Conservative_Backtest.ipynb
    └── Conservative_Backtest.ipynb
```

## Data

The project uses historical NQ futures tick data from the NINJATRADER database (https://ninjatrader.com/). ~90 days of tick data are used for the initial backtest based on periods of low ADX from 2025 to August 2026. This data is the maximum that could be sourced from NINJATRADER's historical dataset. Raw market data is not included in the repository due to file size.

## Disclaimer

This project is for research and educational purposes only and does not constitute financial advice.

