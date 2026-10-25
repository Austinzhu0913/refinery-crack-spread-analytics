# Refinery Crack Spread & Distillate Margin Analytics Engine

An institutional-grade refinery margin analytics engine supporting both **Operational Baseline Monitoring** and **Geopolitical Shock Stress-Testing**, calibrated with official Bloomberg Terminal market settlement standards (`PX_LAST` / `BDP`).

---

## 1. Executive Summary & Dual Scenarios

This analytics engine tracks downstream refinery conversion margins using the industry-standard **3:2:1 Crack Spread**:
- **Feedstock**: 3 barrels of ICE Brent Crude (`CO1 Comdty`)
- **Middle Distillates**: 2 barrels of NYMEX Ultra-Low Sulfur Diesel (`HO1 Comdty`)
- **Light Distillates**: 1 barrel of NYMEX RBOB Gasoline (`XB1 Comdty`)

$$\text{Gross Margin}_{3:2:1} = \frac{2 \times P_{\text{Diesel}} + 1 \times P_{\text{Gasoline}} - 3 \times P_{\text{Brent}}}{3}$$

### Scenario A: Baseline Market Equilibrium (Operational Monitor)
*Default operational run calibrated to normalized refining economics.*
- **ICE Brent Crude**: $76.50 /bbl
- **NYMEX ULSD Diesel**: 242.80 USd/gal ($101.98 /bbl) | Crack: `+$25.48/bbl`
- **NYMEX RBOB Gasoline**: 218.40 USd/gal ($91.73 /bbl) | Crack: `+$15.23/bbl`
- **3:2:1 Gross Margin**: **+$22.06 /bbl** (1Y Mean: **$25.57 /bbl**)

![Baseline Refinery Monitor](crack_spread_baseline.png)

---

### Scenario B: Geopolitical Supply Shock (Bloomberg Terminal Actuals)
*Calibrated directly from Bloomberg Terminal peak settlement records (`PX_LAST` / `PX_SETTLE_LAST_DT`) to evaluate distillate short squeezes and margin elasticities under severe supply cutoffs.*
- **ICE Brent Crude**: $116.72 /bbl (`CO1 Comdty`)
- **NYMEX ULSD Diesel**: 498.42 USd/gal ($209.34 /bbl) | Peak Crack: `+$92.62/bbl`
- **NYMEX RBOB Gasoline**: 321.16 USd/gal ($134.89 /bbl) | Peak Crack: `+$18.17/bbl`
- **Stress-Test 3:2:1 Margin**: **+$67.80 /bbl** (Peak Stress Mean: **$79.28 /bbl**)

![Stress Test Monitor](crack_spread_stress-test.png)

---

## 2. Quantitative Normalization & Units

Commodity contracts on NYMEX and ICE operate under different quoting conventions. The engine handles automated unit harmonization across exchanges:
* **Crude (Brent)**: Quoted in `USD/bbl`. Direct input.
* **Refined Products (ULSD & RBOB)**: Quoted in `cents/gal` (`USd/gal`).
* **Conversion Formula**:
  $$P_{\text{bbl}} = \left( \frac{P_{\text{gal}}}{100} \right) \times 42$$

---

## 3. Project Structure & Usage

.
├── crack.py                     # Dual-mode analytics pipeline
├── crack_spread_baseline.png    # Equilibrium monitor output
├── crack_spread_stress-test.png # Extreme shock stress-test output
└── README.md                    # Methodology and desk documentation

### Quickstart

```bash
# Clone the repository
git clone [https://github.com/](https://github.com/)<your-username>/refinery-crack-spread-analytics.git
cd refinery-crack-spread-analytics

# Run baseline operational monitor
python crack.py --mode baseline

# Run Bloomberg supply-shock stress test
python crack.py --mode stress-test

