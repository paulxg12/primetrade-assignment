# Primetrade.ai — Data Science Assignment

## Candidate: Prince Raymond Paul

---

## Executive Summary

Analyzed **211,224 trades** from **32 Hyperliquid traders** against the **Bitcoin Fear & Greed Index** (2018–2025). The core finding: **market sentiment does not predict trade direction profitability, but strongly predicts trading volume and opportunity size.** Traders who short during Fear capture the highest expected value per trade ($87.68), while greed periods produce the best risk-adjusted returns (profit factor of 7.23).

---

## 1. Data Overview

### Datasets

| Dataset | Rows | Columns | Date Range |
|---|---|---|---|
| Hyperliquid Trader Data | 211,224 | 16 | Mar 2023 – Jun 2025 |
| Fear & Greed Index | 2,644 | 4 | Feb 2018 – May 2025 |
| **Merged (matched dates)** | **184,263** | — | Mar 2023 – May 2025 |

### Hyperliquid Data Structure

| Column | Description |
|---|---|
| Account | 32 unique wallets |
| Coin | 246 unique trading pairs |
| Execution Price | Price at execution |
| Size USD | Trade size in USD (avg: $5,639) |
| Side | BUY (102,696) / SELL (108,528) |
| Direction | Open Long, Close Long, Open Short, Close Short, Sell, Buy, etc. |
| **Closed PnL** | **Primary performance metric** (mean: $48.75, max: $135k, min: -$118k) |
| Fee | Trading fees (avg: $1.16) |
| Timestamp | Unix ms (Mar 2023 – Jun 2025) |

### Fear & Greed Structure

- **Value**: 5 (Extreme Fear) to 95 (Extreme Greed)
- **Classification**: Extreme Fear, Fear, Neutral, Greed, Extreme Greed
- Daily resolution from Feb 2018

---

## 2. Core Analysis: Sentiment vs. Trader Performance

### 2.1 Overall PnL by Sentiment Classification

| Sentiment | Trades | Total PnL | Avg PnL/Trade | Win Rate | Profit Factor |
|---|---|---|---|---|---|
| **Fear** | 133,871 | **$6,699,925** | $50.05 | 42% | **5.97** |
| **Greed** | 36,289 | $3,189,617 | **$87.89** | 45% | **7.23** |
| Extreme Greed | 6,962 | $176,965 | $25.42 | **49%** | 3.22 |
| Neutral | 7,141 | $158,742 | $22.23 | 32% | 1.96 |

**Key Insight**: Fear dominates trading volume (73% of all trades) but Greed produces the **highest average PnL per trade** ($87.89) and best profit factor (7.23). Extreme Greed has the highest win rate (49%) but fewest trades. Neutral markets are the worst — lowest win rate (32%) and worst risk-adjusted returns.

### 2.2 Optimal Fear & Greed Value Ranges

| FG Range | Label | Trades | Total PnL | Avg PnL | Win Rate |
|---|---|---|---|---|---|
| 25–45 | Fear | 133,871 | $6,699,925 | **$50.05** | 42% |
| 55–75 | Greed | 36,289 | $3,189,617 | **$87.89** | **45%** |
| 75–100 | Euphoria | 6,962 | $176,965 | $25.42 | **49%** |
| 45–55 | Neutral | 7,141 | $158,742 | $22.23 | 32% |

**Key Insight**: The "Greed zone" (FG 55–75) is the sweet spot — highest avg PnL per trade with strong volumes. "Fear zone" (25–45) has the highest total PnL due to volume, not edge per trade.

### 2.3 Critical Finding: Short vs. Long by Sentiment

| Sentiment | Position | Avg PnL | Win Rate | Trades | EV/Trade |
|---|---|---|---|---|---|
| **Fear** | **Short** | **$87.68** | 39% | 43,783 | **$87.68** |
| Fear | Long | $35.48 | 44% | 74,383 | $35.48 |
| Greed | Long | $31.66 | 41% | 10,584 | $31.66 |
| Greed | Short | $19.21 | 34% | 10,027 | $19.21 |
| Extreme Greed | Short | $28.35 | 50% | 3,071 | $28.35 |
| Extreme Greed | Long | $24.12 | 50% | 3,728 | $24.12 |
| Neutral | Long | $24.79 | 44% | 1,804 | $24.79 |
| Neutral | Short | $13.09 | 33% | 4,190 | $13.09 |

**Breakthrough Insight**: Shorting during Fear ($87.68/trade) is **~2.5x more profitable** than going long during Fear ($35.48/trade). This is counterintuitive — conventional wisdom says buy the dip. The data shows the best traders are **selling into fear** (shorting during maximum pessimism), not buying it.

During Greed, going long ($31.66) outperforms shorting ($19.21) — but the gap is much narrower.

### 2.4 BUY vs SELL by Sentiment

| Sentiment | Side | Avg PnL | Total PnL | Trades |
|---|---|---|---|---|
| Fear | BUY | **$58.07** | $3,837,630 | 66,081 |
| Fear | SELL | $42.22 | $2,862,296 | 67,790 |
| Greed | **SELL** | **$143.62** | $2,997,016 | 20,868 |
| Greed | BUY | $12.49 | $192,601 | 15,421 |

**Key Insight**: During Greed, **selling is 11.5x more profitable than buying** ($143.62 vs $12.49 avg). This suggests successful traders take profits during euphoria rather than adding to positions.

---

## 3. Trader-Level Insights

### 3.1 Top 5 Traders by Total PnL

| Account | Total PnL | Avg PnL | Win Rate | Avg FG | Trades |
|---|---|---|---|---|---|
| 0xb123... | **$2,040,922** | $141.63 | 34% | 57.67 | 14,410 |
| 0x0833... | **$1,600,230** | $419.13 | 36% | 47.63 | 3,818 |
| 0xbaaa... | **$940,157** | $44.37 | 47% | 44.00 | 21,190 |
| 0xbee1... | **$811,183** | $22.20 | 44% | 52.12 | 36,534 |
| 0x4acb... | **$674,404** | $163.14 | 49% | 46.38 | 4,134 |

**Trader Profile**: The most profitable traders trade predominantly during **Fear** (FG < 50), have win rates **below 50%** but make larger wins than losses.

### 3.2 Trader Sentiment Preferences

- **8 of 32 traders** (25%) trade exclusively during Fear
- The most profitable trader trades across ALL sentiment regimes
- Only **1 trader** has a win rate above 50% (81%)

### 3.3 Monthly Patterns

| Month | Total PnL | Trades | Avg FG |
|---|---|---|---|
| February | $6,699,925 | 133,871 | 44.0 (Fear) |
| October | $3,189,461 | 35,241 | 74.0 (Greed) |
| March | $176,965 | 6,965 | 84.0 (Extreme Greed) |
| July | $158,742 | 7,141 | 50.0 (Neutral) |
| November | $156 | 1,045 | 69.0 (Greed) |

---

## 4. Statistical Correlations

| Relationship | Correlation |
|---|---|
| FG Value vs. Closed PnL | **0.011** (near zero) |
| FG Value vs. |PnL| (trade intensity) | **0.014** (near zero) |
| Trade Size vs. Fear level | Negative: Fear → larger trades ($5,260) vs Greed → smaller trades ($3,183) |

**Critical Finding**: Fear & Greed **does not predict trade outcomes at the individual trade level**. Its predictive power is at the **market regime level** — it tells you WHEN to trade and WHAT strategy to use (short vs. long), not whether any specific trade will win.

---

## 5. Strategic Recommendations

### Strategy 1: "Short Fear, Take Profit in Greed" (Highest Conviction)

| Regime | Action | Expected Value | Rationale |
|---|---|---|---|
| **FG 25–45 (Fear)** | **Go Short** | **$87.68/trade** | Shorting fear is 2.5x better than buying it |
| **FG 55–75 (Greed)** | **Go Long or Sell** | **$31.66/trade (Long)** | Longs win here, but selling is $143.62 avg |
| **FG 75–100 (Euphoria)** | **Take Profits** | $28.35 (Short) | Win rate is 50% — asymmetrically favorable for exits |

### Strategy 2: Avoid Neutral Markets

Neutral sentiment (FG 45–55) has the **worst win rate (32%) and worst profit factor (1.96)**. Reduce position sizing or sit out.

### Strategy 3: Fight the Urge to "Buy the Dip"

Buying during Fear ($35.48) significantly underperforms selling during Fear ($87.68). The best traders provide **liquidity on the short side during panic**, not catching falling knives.

### Strategy 4: Scale With Volatility, Not Conviction

Fear periods have 3.7x the trade volume but Greed produces better PnL per dollar traded.

---

## 6. Methodology

1. **Data Loading**: Both CSVs loaded via pandas
2. **Date Alignment**: Hyperliquid timestamps (Unix ms) converted to dates, merged with Fear & Greed daily values
3. **Filtering**: 26,961 rows (12.8%) dropped due to FG data ending May 2025
4. **Metrics Computed**: Win Rate, Profit Factor, Expected Value
5. **Segmentation**: By classification, value ranges, Side (BUY/SELL), Direction (Long/Short)

### Tools Used
- Python (pandas, numpy)
- Data merging, aggregation, statistical correlation

---

## 7. Caveats & Limitations

1. **Survivorship Bias**: Failed accounts that stopped trading are not captured
2. **Time Window Mismatch**: FG data ends May 2025 (26,961 unmatched rows)
3. **No Risk-Adjusted Metrics**: Without account equity, Sharpe/max drawdown cannot be computed
4. **Classification Skew**: 73% of trades occur during Fear
5. **No Trade Duration**: No entry/exit timestamps per position
6. **Wallet-Level Analysis**: Each account could represent multiple strategies

---

## 8. Further Work

1. Build a predictive model using FG + trader history
2. Cluster traders by strategy (unsupervised learning)
3. Backtest "Short Fear, Long Greed" strategy with slippage/fees
4. On-chain integration for fuller portfolio context
5. Real-time sentiment-strategy dashboard

---

*Submitted for Primetrade.ai internship evaluation — May 2026*
