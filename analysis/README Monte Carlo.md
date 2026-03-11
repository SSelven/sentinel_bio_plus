# 3U CubeSat Orbital Lifetime Monte Carlo Simulation

**Project Title**  
Monte Carlo Analysis of Orbital Lifetime for a 3U CubeSat under Variable Solar and Geomagnetic Conditions

**Overview**  
This repository contains a physics-based Monte Carlo simulation framework developed to estimate the orbital lifetime of a 3U CubeSat launched into low Earth orbit at an initial altitude of approximately 525 km. The model accounts for realistic variations in solar activity (F₁₀.₇ cm radio flux), geomagnetic activity (Ap index including stochastic storm events), atmospheric oblateness, inclination-dependent density corrections, and higher-order gravitational harmonics (J₂, J₃, J₄).

The primary objective is to provide statistically robust lifetime estimates and to quantify the dominant sources of uncertainty — particularly the strong dependence on solar cycle phase and the episodic impact of geomagnetic disturbances.

## Core Capabilities

- High-resolution reference density table derived from NRLMSISE-00 behavior (100–700 km, 1 km spacing)
- Solar activity scenarios: solar minimum (F₁₀.₇ ≈ 70 sfu), solar maximum (F₁₀.₇ ≈ 230 sfu), and continuous random sampling across the cycle
- Geomagnetic activity modeling: background levels + probabilistic storm events with Ap values reaching up to ~400 during extreme events
- Full correction suite:
  - Altitude-dependent solar EUV sensitivity
  - Altitude-dependent geomagnetic heating (Joule + auroral)
  - Equatorial density bulge (~6% enhancement at low inclinations)
  - Auroral heating enhancement at high latitudes
  - Oblate atmosphere correction via EGM96 J₂, J₃, J₄ terms
- 500–1000 Monte Carlo realizations per scenario → distribution statistics, percentiles, confidence bands
- Comprehensive diagnostic visualization library (>30 figures)

## Representative Lifetime Estimates  
(Example values — actual results depend on random seed and parameter settings)

| Inclination | Solar Minimum Mean [5th–95th] | Mean Ap (quiet) | Solar Maximum Mean [5th–95th] | Mean Ap (active+storms) | Lifetime Ratio (Min/Max) |
|-------------|--------------------------------|------------------|--------------------------------|---------------------------|---------------------------|
| 97.3° (SSO) | 11.8 years [7.1 – 18.2]        | ~4.2            | 2.4 years [0.9 – 5.1]          | ~28                      | ≈ 4.9×                   |
| 60.6°       | 10.1 years [6.0 – 15.9]        | ~4.2            | 2.1 years [0.8 – 4.6]          | ~26                      | ≈ 4.8×                   |
| 40.1°       | 9.4 years  [5.5 – 14.7]        | ~4.2            | 2.0 years [0.7 – 4.3]          | ~25                      | ≈ 4.7×                   |

**Key Insight**  
At 525 km altitude, solar cycle variability alone produces a factor of ~4.5–5× difference in expected mean lifetime. Geomagnetic storms introduce additional episodic risk capable of reducing lifetime by 20–60% in affected realizations.

## Technical Requirements

- Python 3.8+
- Required packages:
  - numpy
  - scipy (for `solve_ivp`)
  - matplotlib
- Optional: tqdm (progress visualization during long Monte Carlo runs)

No external atmospheric model interface or license is required — all density and correction physics are self-contained.

## Quick Execution

```bash
# Optional: adjust mission parameters at the top of the script
#   MASS, AREA, CD, INITIAL_ALTITUDE, INCLINATIONS, N_SIMULATIONS, etc.

python cubesat_lifetime_mc.py
```

**Expected runtime**  
≈ 8–30 minutes on modern hardware (dominant cost is numerical integration of the orbital decay ODE across thousands of realizations).

**Outputs**  
- Model validation report
- Tabular statistical summary
- Comprehensive set of publication-quality diagnostic figures

## Important Modeling Limitations

- Simplified density model — omits explicit local solar time, day-of-year, and semi-annual variations
- Constant ballistic coefficient assumed (no attitude dynamics or tumbling)
- No orbit maintenance, drag makeup, or propulsive maneuvers modeled
- No third-body (Moon/Sun), albedo, or infrared radiation pressure perturbations
- Geomagnetic storm occurrence is stochastic and may not perfectly reproduce real historical statistics

This tool is intended for **trade-off studies**, **uncertainty quantification**, **mission concept evaluation**, and **educational/research purposes**. It is not a certified operational orbit prediction engine. For flight-critical applications, use validated high-fidelity propagators with official space weather inputs.

## License

MIT License

## Primary References

- Picone et al. (2002) — NRLMSISE-00 empirical model  
- Vallado (2013) — *Fundamentals of Astrodynamics and Applications* (4th ed.)  
- Doornbos (2012) — *Thermospheric Density and Wind Determination from Satellite Dynamics*  
- Emmert et al. (2008) — Thermospheric density variations during geomagnetic storms  
- Lemoine et al. (1998) — EGM96 Earth gravity model  
- Oltrogge & Leveque (2011) — Evaluation of CubeSat orbital decay behavior  
- NOAA Space Weather Prediction Center — Kp/Ap and F₁₀.₇ data archives

This simulation framework provides a transparent, reproducible, and computationally accessible means of exploring the first-order lifetime sensitivities that govern the operational viability of unpropelled CubeSats in low Earth orbit.
  
