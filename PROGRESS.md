# VeighNa Project Progress

## Environment

- Platform: Windows 11
- Python: 3.11
- vnpy version: 4.3.0

---

## Setup Status

| Component | Status | Notes |
|-----------|--------|-------|
| vnpy core | ✅ Installed | `pip install -e .` |
| PySide6 GUI | ✅ Working | Basic GUI launches via `examples/veighna_trader/run_basic.py` |
| VS Build Tools | ⏳ Installing | Required to compile `vnpy_ctp` |
| vnpy_ctp | ❌ Pending | Blocked on VS Build Tools C++ compiler |
| vnpy_ctastrategy | ❌ Pending | |
| vnpy_ctabacktester | ❌ Pending | |
| vnpy_datamanager | ❌ Pending | |

---

## ~~TQSDK Data Access~~ — Abandoned

> **Reason**: TQSDK historical data requires a paid subscription. Switched to RQData.

### TQSDK Summary

- [x] Installed and connected successfully
- ❌ Abandoned — historical data access is not free

---

## RQData Data Source — Active ✅

**Purchased and working.** Configured in `~/.vntrader/vt_setting.json` with `datafeed.name = rqdata`.

### Steps

- [x] Purchase RQData subscription
- [x] Install RQData client: `pip install rqdatac`
- [x] Install vnpy integration: `pip install vnpy_rqdata`
- [x] Test connection and authenticate — **working as expected** ✅
- [ ] Download historical bar data for all commodity contracts
- [ ] Run a backtest using the downloaded data

---

## Current Focus: CTP Live Trading Setup

**Goal**: Pass CTP穿透式监管 certification testing with broker (上海中期, BrokerID `8070`) to enable live trading.

### Broker Info

| Field | Value |
|-------|-------|
| Broker | 上海中期期货 (shcifco) |
| BrokerID | `8070` |
| Trade Server | `zjzx-front1.ctp.shcifco.com:21205` |
| Market Data Server | `zjzx-md1.ctp.shcifco.com:21213` |

### Steps

- [ ] Finish VS Build Tools installation → compile and install `vnpy_ctp`
- [ ] Install `vnpy_ctastrategy`, `vnpy_ctabacktester`, `vnpy_datamanager`
- [ ] Launch full GUI via `examples/veighna_trader/run.py`
- [ ] Complete CTP穿透式监管 certification test with 上海中期
- [ ] Connect to live broker and verify market data feed
- [ ] Place a test order to confirm order routing

---

## Issues Log

| Date | Issue | Resolution |
|------|-------|------------|
| 2026-02-24 | `vnpy_ctp` fails to install - no C++ compiler | Installing VS Build Tools 2022 |
| 2026-02-24 | TQSDK connection established | ✅ Resolved |
| 2026-02-24 | TQSDK historical data requires paid subscription | Switched to RQData |
| 2026-02-25 | RQData purchased and working | ✅ Active data source |

---

## Next Steps

1. Finish VS Build Tools installation → install `vnpy_ctp`
2. Complete CTP穿透式监管 certification test with 上海中期
3. Connect to live broker and verify market data and order routing
4. Download full historical data via RQData for all commodity contracts
5. Run a simple CTA strategy backtest
