# smart_order_router
Task Provided by blockhouse
# Smart Order Router Backtest – Cont & Kukanov Cost Model

This task implements a backtest for a Smart Order Router that aims to execute a 5,000 share buy order across multiple trading venues efficiently using the static cost model proposed by **Cont & Kukanov**.

## Objective

Minimize the total execution cost and risk by optimally splitting the order based on:
- **Market conditions such as ask price & size**
- **Penalties for underfilling, overfilling, and queue risks**
- **Fees and rebates per venue**

The model is benchmarked against common baseline strategies:
- **Best-Ask**: Greedy approach, always buy from the venue with the lowest ask
- **TWAP (Time-Weighted Average Price)**: Equal share allocation over time
- **VWAP (Volume-Weighted Average Price)**: Allocation based on displayed liquidity

## Components

### `backtest.py`
- Reads `l1_day.csv` and processes market snapshots
- Implements allocation optimizer using Cont & Kukanov's cost function
- Compares against baseline strategies (Best-Ask, TWAP, VWAP)
- Plots cumulative cost over time (`results.png`)
- Prints summary statistics in JSON format

## JSON Output

```
{
  "best_parameters": {
    "lambda_over": 0.01,
    "lambda_under": 0.05,
    "theta_queue": 0.001
  },
  "optimized_result": {
    "total_cash_spent": 1113715.0,
    "avg_fill_price": 222.743
  },
  "best_ask_baseline": {
    "total_cash_spent": 1114102.28,
    "avg_fill_price": 222.8205,
    "savings_bps": 3.48
  },
  "twap_baseline": {
    "total_cash_spent": 1113783.57,
    "avg_fill_price": 222.7567,
    "savings_bps": 0.62
  },
  "vwap_baseline": {
    "total_cash_spent": 1114102.28,
    "avg_fill_price": 222.8205,
    "savings_bps": 3.48
  }
}
```
## Parameters Tuned
- `lambda_over`: Overfill penalty
- `lambda_under`: Underfill penalty
- `theta_queue`: Queue risk penalty

Values are selected through a grid search over a small range for simplicity.

## Files included
- `backtest.py` – Main backtest script
- `results.png` – Visualization of cumulative cost

## How to Run

```
python backtest.py
```
Please ensure `l1_day.csv` is in the same directory. The script outputs JSON to the terminal and generates a `results.png` file.
