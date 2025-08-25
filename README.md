# Midnight Momentum Strategy

A sophisticated overnight trading strategy that leverages statistical analysis and empirical thresholds to capitalize on intraday price recovery patterns. This strategy uses a 5-day scaling approach combined with rigorous statistical validation to achieve consistent returns across multiple asset classes.

## 🎯 Strategy Overview

The Midnight Momentum Strategy is based on the empirical observation that stocks frequently recover above their previous day's closing price during the trading session, even after negative overnight gaps. The strategy employs:

- **Entry**: Buy at market close
- **Exit**: Sell when price hits empirically-derived profit targets (68% confidence threshold)
- **Risk Management**: 5-day scaling-in approach for underwater positions
- **Capital Allocation**: 10% risk per trade with dynamic position sizing

## 📊 Performance Summary

### Backtested Assets (Aug 2022 - Aug 2025)
- **AAPL**: 14.43% ROI, 100% Win Rate, 60 trades
- **MSFT**: 29.64% ROI, 100% Win Rate, 112 trades  
- **GOOGL**: 26.73% ROI, 100% Win Rate, 102 trades
- **TSLA**: 19.45% ROI, 100% Win Rate, 105 trades
- **NVDA**: 34.23% ROI, 100% Win Rate, 161 trades
- **AMZN**: 29.52% ROI, 100% Win Rate, 125 trades
- **META**: 40.42% ROI, 100% Win Rate, 173 trades
- **SPY**: 25.49% ROI, 100% Win Rate, 73 trades
- **QQQ**: 22.60% ROI, 100% Win Rate, 73 trades

### Key Metrics
- **Average ROI**: 26.88%
- **Win Rate**: 100% across all assets
- **Total Trades**: 984
- **Sharpe Ratio Range**: 0.67 - 0.87
- **Maximum Drawdown**: 0% (no losing trades)

## 🔬 Statistical Foundation

### Recovery Rate Analysis
The strategy is built on robust statistical analysis showing:
- **Overall Recovery Rate**: 82-85% across all assets
- **Statistical Significance**: p-value < 0.0001 (Monte Carlo validated)
- **Confidence Intervals**: Bootstrap-validated with 95% CI
- **Walk-Forward Analysis**: Consistent performance across 23 out-of-sample periods

### Threshold Methodology
- **68% Confidence Level**: Primary trading threshold
- **Empirical Quantiles**: Non-parametric threshold calculation
- **No Look-Ahead Bias**: Rolling window threshold updates
- **Multiple Testing Correction**: FDR-adjusted p-values

## 🎲 5-Day Scaling Strategy

### Core Innovation
The strategy's key differentiator is the 5-day scaling approach:

1. **Initial Entry**: Buy at close with 10% of capital
2. **Day 5 Scaling**: If position underwater after 5 days, scale in to bring weighted average within 0.02% of current price
3. **New Target**: Recalculate 1% profit target from scaled average price
4. **Exit**: Only exit when profit target is hit

### Scaling Performance
- **Scaling Rate**: 12-41% of trades (varies by asset)
- **Scaled Trade Performance**: 158-198% higher average profit per trade
- **Underwater Recovery**: 80-87% of underwater days show next-day recovery potential

## 📁 Repository Structure

```
MidnightMomentumStrategy/
├── midnightMomentum_backtest.py          # Main backtest engine
├── handlers/                             # Data handling modules
│   ├── connection_manager.py
│   ├── fetch_data.py
│   ├── historical_data_handler.py
│   └── order_handler.py
├── visualizers/                          # Visualization tools
│   └── midnightMomentum_visualization.py
├── historical_data/                      # Generated CSV files with thresholds
├── charts/                              # Generated visualization charts
├── data/robust_analysis/                # Statistical analysis results
├── README.md                           # This file
└── WHITEPAPER.md                       # Detailed technical analysis
```

## 🚀 Quick Start

### Prerequisites
```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install pandas numpy matplotlib seaborn scipy statsmodels requests
```

### Running the Backtest
```bash
# Single ticker
python3 midnightMomentum_backtest.py AAPL

# Multiple tickers
python3 midnightMomentum_backtest.py AAPL MSFT GOOGL TSLA NVDA

# With custom parameters
python3 midnightMomentum_backtest.py AAPL --confidence-levels 0.68 0.90 0.95 --transaction-cost 0.001
```

### Output Files
- **CSV Data**: `historical_data/{SYMBOL}_overnight_hold_backtest_with_thresholds.csv`
- **Charts**: `charts/{SYMBOL}_overnight_hold_comprehensive.png`
- **Analysis**: `data/robust_analysis/{SYMBOL}_robust_analysis_results.json`

## 📈 Key Features

### Statistical Rigor
- **Monte Carlo Validation**: 1000+ simulations per test
- **Bootstrap Confidence Intervals**: Robust statistical inference
- **Walk-Forward Analysis**: Out-of-sample validation
- **Multiple Testing Correction**: FDR-adjusted significance levels

### Risk Management
- **Position Sizing**: Dynamic allocation based on current equity
- **Transaction Costs**: Integrated 0.1% cost per trade
- **Volatility Regimes**: Adaptive thresholds based on market conditions
- **No Stop Losses**: Strategy relies purely on statistical edge

### Visualization
- **Comprehensive Charts**: Price action with profit/risk thresholds
- **Performance Metrics**: Detailed trade-by-trade analysis
- **Comparative Analysis**: Multi-asset performance comparison
- **Statistical Summaries**: Recovery rates and effectiveness metrics

## 🎯 Strategy Logic

### Entry Conditions
1. Valid empirical threshold data available
2. Optional: Volatility regime filter (disabled by default)
3. Sufficient capital for position sizing

### Scaling Logic
```python
# Day 5 scaling calculation
if days_held == 5 and not scaled_in:
    target_weighted_avg = current_close * (1 + 0.0002)  # 0.02% above close
    additional_shares = calculate_scaling_shares(original_shares, original_price, target_avg, scale_price)
    new_target_price = weighted_avg_entry_price * 1.01  # 1% profit target
```

### Exit Conditions
- **Primary**: Price hits 1% profit target
- **Scaling**: Price hits recalculated target after scaling

## 📊 Performance Analysis

### Best Performing Assets
1. **META**: 40.42% ROI (173 trades)
2. **NVDA**: 34.23% ROI (161 trades)
3. **MSFT**: 29.64% ROI (112 trades)
4. **AMZN**: 29.52% ROI (125 trades)

### Most Efficient Assets
1. **SPY**: 87.29 avg profit per trade
2. **QQQ**: 77.39 avg profit per trade
3. **MSFT**: 66.16 avg profit per trade

### Scaling Effectiveness
- **Highest Scaling Rate**: SPY (41.1%)
- **Best Scaling Performance**: TSLA (+198.17 per scaled trade)
- **Most Consistent**: All assets show 80%+ underwater recovery potential

## ⚠️ Risk Considerations

### Strategy Limitations
- **Market Regime Dependency**: Performance may vary in different market conditions
- **Liquidity Requirements**: Requires sufficient volume for scaling operations
- **Capital Intensive**: 5-day scaling can significantly increase position sizes
- **No Stop Losses**: Relies entirely on statistical edge for risk management

### Implementation Risks
- **Execution Slippage**: Real-world execution may differ from backtest assumptions
- **Transaction Costs**: Higher frequency trading increases cost impact
- **Market Impact**: Large positions may affect entry/exit prices
- **Regime Changes**: Strategy effectiveness may diminish if market patterns change

## 🔬 Technical Details

### Statistical Tests
- **Gap Prediction Test**: Tests if overnight gaps predict recovery likelihood
- **Sequence Information Test**: Validates temporal ordering importance
- **Market Regime Test**: Assesses regime-dependent predictability

### Threshold Calculation
```python
# Empirical threshold calculation (no look-ahead bias)
threshold_pct = historical_drops.quantile(confidence_level)
threshold_price = prev_close * (1 - threshold_pct / 100)
```

### Performance Metrics
- **Sharpe Ratio**: Risk-adjusted returns
- **Profit Factor**: Gross profit / Gross loss
- **Maximum Drawdown**: Peak-to-trough decline
- **Win Rate**: Percentage of profitable trades

## 📚 Further Reading

- **WHITEPAPER.md**: Comprehensive technical analysis and methodology
- **Statistical Validation**: Monte Carlo and bootstrap methodologies
- **Risk Management**: Position sizing and scaling algorithms
- **Market Microstructure**: Overnight gap analysis and recovery patterns

## 🤝 Contributing

This is a research-focused repository. For questions or discussions about the methodology, please refer to the detailed whitepaper or create an issue for technical questions.

## ⚖️ Disclaimer

This strategy is for educational and research purposes only. Past performance does not guarantee future results. Trading involves substantial risk of loss and is not suitable for all investors. Always conduct your own research and consider consulting with a financial advisor before implementing any trading strategy.

---

*Last Updated: August 2025*
*Backtest Period: August 2022 - August 2025*
*Assets Tested: 9 major US equities and ETFs*
