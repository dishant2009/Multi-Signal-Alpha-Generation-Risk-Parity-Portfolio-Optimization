# Multi Signal Alpha Generation Risk Parity Portfolio Optimization
## Overview

A comprehensive quantitative trading system that combines traditional factor models with alternative data sources to generate alpha signals and construct risk-optimized portfolios. The system achieves consistent risk-adjusted outperformance through sophisticated signal blending, dynamic risk management, and efficient execution.

## Key Features

- **Multi-Signal Alpha Generation**: Momentum, mean reversion, quality factors, volatility risk premium, and alternative data signals
- **Advanced Portfolio Optimization**: Hierarchical Risk Parity (HRP) and Black-Litterman with alpha overlay
- **Dynamic Risk Management**: Real-time volatility targeting and drawdown control
- **Smart Execution**: VWAP, TWAP, and adaptive algorithms with transaction cost optimization
- **Comprehensive Monitoring**: Real-time performance tracking, alerting, and reporting
- **Production-Ready**: Docker deployment, Airflow scheduling, and horizontal scaling

## Architecture

```
alpha-system/
├── data/                  # Data loading and processing
│   ├── loaders.py        # API integrations (Yahoo, FRED, Google Trends)
│   └── processors.py     # Data cleaning and feature engineering
├── signals/              # Alpha signal generation
│   ├── momentum.py       # Momentum and mean reversion
│   ├── quality.py        # Fundamental quality metrics
│   └── alternative.py    # Alternative data signals
├── portfolio/            # Portfolio construction
│   ├── optimization.py   # HRP and Black-Litterman
│   ├── risk_mgmt.py     # Risk controls and limits
│   └── execution.py     # Order management system
├── monitoring/           # System monitoring
│   ├── metrics.py       # Performance analytics
│   ├── alerts.py        # Alert management
│   └── reports.py       # Report generation
├── tests/               # Comprehensive test suite
├── airflow_dags/        # Production scheduling
├── config/              # Configuration files
└── deployment/          # Docker and Kubernetes configs
```

## Installation

### Prerequisites

- Python 3.9+
- Docker and Docker Compose
- PostgreSQL 14+
- Redis 7+
- (Optional) Kubernetes cluster for production deployment


### Quick Start

1. Clone the repository:
```bash
git clone https://github.com/dishant2009/Multi-Signal-Alpha-Generation-Risk-Parity-Portfolio-Optimization.git
cd alpha-system
```

2. Copy environment variables:
```bash
cp .env.example .env
# Edit .env with your API keys and credentials
```

3. Build and start services:
```bash
make build
make deploy
```

4. Access the services:
- Main Application: http://localhost:8000
- Airflow UI: http://localhost:8081
- Monitoring Dashboard: http://localhost:8080
- Grafana: http://localhost:3000
- Jupyter Lab: http://localhost:8888

## Configuration

The system is configured via `config.yaml`:

```yaml
data:
  universe:
    method: "market_cap"
    top_n: 500
    
portfolio:
  optimization:
    method: "hierarchical_risk_parity"
    constraints:
      max_position: 0.05
      
  risk_targets:
    volatility: 0.12
    max_drawdown: 0.15
    
monitoring:
  alerts:
    drawdown_threshold: 0.10
    sharpe_decline: 0.5
```
## Usage

### Running Backtests

```python
from alpha_system import BacktestingEngine, DataLoader

# Initialize
data_loader = DataLoader(start_date='2015-01-01', end_date='2023-12-31')
engine = BacktestingEngine(data_loader)

# Run backtest
results = engine.run_backtest(
    tickers=['AAPL', 'MSFT', 'GOOGL', ...],
    rebalance_freq='M',
    transaction_cost=0.001
)

# Visualize results
engine.plot_results()
```

### Generating Signals

```python
from alpha_system.signals import AlphaSignals, SignalCombiner

# Generate individual signals
alpha_gen = AlphaSignals(returns, prices)
signals = {
    'momentum': alpha_gen.momentum_12_1(),
    'quality': alpha_gen.quality_score(),
    'volatility': alpha_gen.volatility_risk_premium()
}

# Combine with IC weighting
combiner = SignalCombiner(returns)
combined_signals = combiner.calculate_ic_weighted_signals(signals)
```

### Portfolio Optimization

```python
from alpha_system.portfolio import PortfolioOptimizer

# Initialize optimizer
optimizer = PortfolioOptimizer(returns, alpha_signals)

# Run optimization
weights = optimizer.hierarchical_risk_parity()

# Or use Black-Litterman
weights = optimizer.black_litterman_optimization(market_caps)
```

