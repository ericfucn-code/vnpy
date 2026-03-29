# Claude Code Instructions — VeighNa (vnpy) Project

## Skill Files — Read Before Working

Before starting any task in this project, load the relevant skill files:

| Task | Skill File to Read First |
|------|--------------------------|
| Any backtesting design, evaluation, or result analysis | [`backtesting_evaluation.md`](C:/Users/ericf/.claude/projects/e--Projects-vnpy/backtesting_evaluation.md) |
| Any vnpy code change, alpha research, or futures workflow | [`vnpy_reference.md`](C:/Users/ericf/.claude/projects/e--Projects-vnpy/vnpy_reference.md) |

Both files live at: `C:/Users/ericf/.claude/projects/e--Projects-vnpy/`

---

## Project Overview

This is a quantitative trading research project built on **VeighNa (vnpy) 4.3.0** targeting **China commodity futures**.

- **Platform**: Windows 11, Python 3.11
- **Primary focus**: Alpha factor research (Alpha158 → LightGBM → signal → backtest)
- **Data sources**: AKShare (free, daily) + RQData (paid, adjusted continuous contracts)
- **Broker**: 上海中期 (shcifco), BrokerID 8070, CTP API

---

## Rules (Always Follow)

### 1. Never patch site-packages
If a fix is needed in a package from site-packages, tell the user to clone the git repo first. All used packages are already cloned at editable install locations:

| Package | Editable location |
|---------|-------------------|
| `vnpy` core | `e:/Projects/vnpy` |
| `vnpy_ctastrategy` | `e:/Projects/vnpy_ctastrategy` |
| `vnpy_ctabacktester` | `e:/Projects/vnpy_ctabacktester` |
| `vnpy_rqdata` | `e:/Projects/vnpy_rqdata` |

### 2. Futures backtesting engine
- **DO NOT** use `vnpy.alpha.strategy.BacktestingEngine` for futures — it is equity-only (no multiplier, no margin, no short positions)
- **USE** `vnpy_ctastrategy.backtesting.BacktestingEngine` for futures

### 3. prepare_data in Jupyter on Windows
Always use `max_workers=1`:
```python
dataset.prepare_data(filters=None, max_workers=1)
```
Reason: `get_context("spawn").Pool(processes=N)` always spawns child processes. On Windows Jupyter, the spawn context re-imports `__main__` (the notebook), corrupting Polars DataFrames in the worker. The fix in `template.py` bypasses Pool entirely when `max_workers=1`.

### 4. Backtesting evaluation (read skill file first)
Before writing any backtest evaluation code or interpreting results, read `backtesting_evaluation.md`. Key rules:
- Always generate: equity curve + drawdown, monthly heatmap, rolling Sharpe
- Always compute: Sharpe, Sortino, Calmar, Max DD, profit factor, win rate, expectancy
- Always run: outlier sensitivity (remove best 5 trades), Monte Carlo (5000 simulations)
- China futures: use `periods_per_year=242`, risk-free rate ≈ 2.5%
- A Sharpe > 3.0 on daily data is a **red flag** — check for look-ahead bias

### 5. Symbol conventions
- vnpy uses `88`-suffix for main continuous contracts: `rb88.SHFE`, `cu88.SHFE`
- AKShare uses `0`-suffix: `RB0`, `CU0`
- Use `SINA_SYMBOL_MAP` to bridge them (defined in `research_workflow_futures.ipynb`)
- RQData auto-resolves 88-suffix and applies `adjust_type="pre"` for continuous contracts

---

## Key Architecture Changes Made

These are permanent modifications — do NOT revert:

1. **`Interval.MINUTE_15 = "15m"`** added to `vnpy/trader/constant.py`
2. **`INTERVAL_DELTA_MAP`** in `vnpy_ctastrategy/base.py` includes `Interval.MINUTE_15`
3. **`INTERVAL_VT2RQ`** in `vnpy_rqdata/rqdata_datafeed.py` includes `Interval.MINUTE_15`
4. **`prepare_data`** in `vnpy/alpha/dataset/template.py` bypasses Pool for `max_workers=1`
5. **`show_feature_performance`** uses `quantiles=5, max_loss=1.0` (futures have fewer assets than equities)
6. **RQData adjust_type**: 88/888/99-suffix contracts use `adjust_type="pre"` in `rqdata_datafeed.py`

---

## Main Research Files

| File | Purpose |
|------|---------|
| `examples/alpha_research/research_workflow_futures.ipynb` | Full end-to-end futures alpha workflow |
| `vnpy/alpha/lab.py` | Data warehouse (parquet storage) |
| `vnpy/alpha/dataset/template.py` | AlphaDataset (features, `prepare_data`) |
| `vnpy/alpha/dataset/datasets/alpha_158.py` | 158 OHLCV features + label definition |
| `vnpy/alpha/model/models/lgb_model.py` | LightGBM model |
