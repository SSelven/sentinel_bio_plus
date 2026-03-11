The **NRLMSISE-00** (Naval Research Laboratory Mass Spectrometer and Incoherent Scatter Radar Exosphere – year 2000) is currently one of the most widely used **empirical global reference atmospheric models** for Earth's atmosphere. It describes neutral atmospheric conditions from the ground (0 km) up to the lower exosphere/exobase region (~1000 km altitude).

### Core Purpose and Scope

NRLMSISE-00 provides:

- **Total neutral mass density** (kg/m³) — most important quantity for satellite drag / orbital lifetime calculations
- **Temperature** (neutral, kinetic, and exospheric temperature)
- **Number densities** (particles/m³) of individual neutral species:
  - He, H, N, O, Ar, N₂, O₂
  - "Anomalous oxygen" component (mainly for drag above ~500 km — represents hot atomic oxygen + ionized oxygen contributions)

The model covers the **entire altitude range** from the troposphere through the mesosphere, thermosphere, and into the exosphere — making it one of the few models that spans ground-to-space in a single formulation.

### Historical Context & Development

- **Predecessors**:
  - MSIS-86 (Hedin, 1987)
  - MSISE-90 (Hedin, 1991) — extended MSIS downward to the ground
- **Major 2002 upgrade** (Picone et al., Journal of Geophysical Research, 2002):
  - Authors: J.M. Picone, A.E. Hedin, D.P. Drob, A.C. Aikin
  - Added large new datasets that were not used in earlier MSIS versions:
    - Satellite accelerometer total mass density
    - Satellite orbital drag-derived density (including Jacchia/Barlier data)
    - Incoherent scatter radar temperatures
    - Molecular oxygen (O₂) densities from Solar Maximum Mission (SMM) UV occultation
  - Introduced the **"anomalous oxygen"** term to explicitly account for O⁺ and hot atomic O contributions to drag at very high altitudes (>500 km)

This made NRLMSISE-00 significantly more accurate for thermospheric density and satellite drag applications compared to MSISE-90 and the older Jacchia-70 models.

### Key Input Drivers (Space Weather & Geometry)

The model responds to:

- **Solar activity** — parameterized mainly by F₁₀.₇ cm radio flux (daily and 81-day average values)
- **Geomagnetic activity** — parameterized by Ap or Kp indices (3-hourly values)
- **Geographical & temporal coordinates**:
  - Altitude
  - Latitude
  - Longitude
  - Day of year
  - Universal time (or local solar time)
- Optional flags for including/outcluding certain species or terms

### Mathematical & Physical Character

NRLMSISE-00 is **semi-empirical**:

- Heavily data-driven (large historical database of measurements)
- Constrained by physical principles:
  - Hydrostatic equilibrium
  - Diffusive equilibrium above the turbopause (~100–110 km)
  - Thermal diffusion and other transport processes
- Uses analytical functional forms with many fitted coefficients (several hundred parameters total)
- Temperature profile follows a **Bates-type formulation** in the thermosphere
- Species densities transition from well-mixed behavior (below ~100 km) to diffusive separation above the turbopause

### Principal Applications

1. **Satellite orbital drag & lifetime prediction** — most common use
2. **Upper atmosphere research** (climatology, response to space weather)
3. **Reentry analysis**
4. **Astronomical extinction corrections** (very high-altitude neutral density)
5. **Initialization / boundary conditions** for first-principles general circulation models

### Strengths

- Very good representation of solar and geomagnetic variability
- Includes anomalous oxygen — important for drag above ~500–600 km
- Widely benchmarked and still the **de facto standard** in many orbit propagation tools (STK, Orekit, GMAT, Basilisk, etc.)
- Freely available FORTRAN/C implementations (CCMC, NASA, etc.)

### Known Limitations

- Climatological — represents **average / expected** conditions, not real-time weather
- No explicit local solar time / diurnal tide beyond what is captured by F₁₀.₇ and Ap drivers
- Uncertainty grows above ~600–700 km (especially during very low solar activity)
- Does not include day-to-day or short-period wave variability
- "Anomalous oxygen" term is semi-empirical and sometimes debated


# NRLMSISE-00 vs. NRLMSIS 2.0  
**A Practical Comparison for Satellite Drag & Orbital Lifetime Applications**

NRLMSISE-00 (2002) and **NRLMSIS 2.0** (2020–2021) are successive generations of the Naval Research Laboratory's empirical Mass Spectrometer and Incoherent Scatter Radar (MSIS) neutral atmosphere models. Both are among the most widely used reference models for thermospheric density in orbital mechanics, satellite drag prediction, re-entry analysis, and upper-atmosphere research.

NRLMSIS 2.0 is **not** a minor patch — it is a major architectural reformulation with significantly improved physical consistency and different density scaling, especially in the thermosphere.

## Quick Summary Table

| Feature / Property                        | NRLMSISE-00 (2002)                                      | NRLMSIS 2.0 (2020/2021)                                      | Practical Impact for Orbital Lifetime & Drag Prediction                  |
|-------------------------------------------|----------------------------------------------------------------|---------------------------------------------------------------------|--------------------------------------------------------------------------|
| Vertical domain                           | 0 – ~1000 km                                                   | 0 – ~1000 km                                                        | Same coverage                                                            |
| Vertical coordinate                       | Geometric altitude                                             | **Geopotential height**                                             | Small difference above ~500 km; more physically consistent               |
| Whole-atmosphere coupling                 | Thermosphere largely decoupled from lower atmosphere            | **Full hydrostatic/diffusive coupling** from ground upward          | Major structural improvement; much better MLT region consistency         |
| Atomic oxygen (O) lower boundary          | ~85–100 km                                                     | Extended down to **50 km**                                          | Better mesosphere–lower thermosphere behavior                            |
| New assimilated datasets                  | Accelerometers, orbital drag, ISR, SMM O₂, anomalous O         | **Extensive new** lower/middle atmosphere T, O, H + updated drag    | Significantly reduced biases below ~120 km                               |
| Thermospheric N₂ density                  | Baseline                                                       | **~18% lower** on average                                           | Lower total mass density in mid/upper thermosphere                       |
| Thermospheric atomic oxygen (O) density   | Baseline + anomalous O term                                    | **~10% lower** overall                                              | Noticeably lower drag at 400–700 km                                      |
| Total neutral mass density (typical LEO)  | Reference for most legacy studies                              | **~10–20% lower** at 400–600 km (varies with solar activity)       | → Shorter predicted lifetimes when switching from -00 to 2.0             |
| Anomalous oxygen term                     | Explicit above ~500 km                                         | Retained, but less dominant due to lower base species               | Still corrects drag at very high altitudes                               |
| Mesosphere & lower thermosphere residuals | Larger biases/scatter                                          | **Much lower** residuals                                            | Preferred for sounding rockets, aerobraking, MLT science                 |
| Stratosphere / upper troposphere          | Warmer stratosphere in some regimes                            | **Warmer troposphere**, **cooler stratosphere & mesosphere**        | Improved lower-atmosphere climatology                                    |
| License / commercial use                  | Very permissive (included in Orekit, STK, GMAT, Basilisk, etc.)| **Restrictive** — requires written permission from NRL for commercial use | Major barrier for commercial/orbital operations software                 |
| Typical computational cost                | Faster                                                         | **30–50% slower** (more parameters + stronger coupling)             | Still fast enough for Monte Carlo and real-time propagation              |
| Current adoption (2026)                   | Still the **de facto engineering standard** in many tools      | Preferred in academic/research; slower adoption in operations       | -00 remains dominant in industry & legacy pipelines                      |

## Key Scientific & Structural Differences

1. **Full-atmosphere hydrostatic equilibrium**  
   NRLMSIS 2.0 enforces consistent pressure, temperature, and composition from the troposphere to the exobase.  
   NRLMSISE-00 joined separate lower- and upper-atmosphere profiles somewhat artificially → leads to inconsistencies in the mesosphere–lower thermosphere (MLT) region.

2. **Lower thermospheric density reduction**  
   The cooler mesosphere in 2.0 reduces upward expansion → **systematically lower** N₂ and O densities in the thermosphere.  
   Combined with updated orbit-derived drag data, total mass density is typically **10–20% lower** at CubeSat/LEO altitudes.

3. **Improved data foundation**  
   2.0 assimilates much more modern and diverse lower/middle atmosphere measurements (temperature, O, H) → dramatically better performance below ~120 km.

4. **License restriction**  
   NRLMSIS 2.0 is **not** freely redistributable for commercial purposes without explicit NRL approval — this has significantly slowed its adoption in commercial flight software compared to the very permissive NRLMSISE-00.

## Implications for Orbital Lifetime & Satellite Drag

| Scenario / Use Case                               | NRLMSISE-00 Behavior                          | NRLMSIS 2.0 Behavior                          | Recommendation (2026)                                 |
|---------------------------------------------------|-----------------------------------------------|-----------------------------------------------|-------------------------------------------------------|
| CubeSat / smallsat lifetime at 400–600 km         | Longer predicted lifetimes                    | **Shorter** by ~5–20% (lower density)         | Use 2.0 if possible; otherwise add margin on -00     |
| Legacy validation / certification                 | Matches most historical tools & papers        | Requires re-baselining                        | Stick with -00 for continuity                         |
| Commercial/orbital operations software            | Permissive license, widely integrated         | License restrictions                          | NRLMSISE-00 is still the safer default                |
| Academic/research (MLT region, sounding rockets)  | Acceptable but dated                          | **Clearly superior** below ~150 km            | Prefer NRLMSIS 2.0 (or 2.1)                           |
| Extreme solar minimum / very low density          | May overestimate density                      | Better captures low-density regime            | 2.0 usually more realistic                            |
| High solar activity / geomagnetic storms          | Robust, widely benchmarked                    | Similar thermospheric response                | Either model acceptable (margins dominate)            |

**Rule of thumb (2026):**  
- If your tool chain or contract already uses NRLMSISE-00 → **stay with it** and apply conservative margins (especially during solar maximum).  
- If starting fresh or doing academic/science work → **use NRLMSIS 2.0** (or the later 2.1 release) for better physical realism and lower-atmosphere accuracy.  
- If commercial constraints apply → NRLMSISE-00 is usually the path of least resistance.

Both models remain climatological — neither captures real-time day-to-day variability. For highest-fidelity drag prediction, many modern operations teams use **multi-model ensembles** (NRLMSISE-00 + NRLMSIS 2.0 + JB2008/DTM-2020 + HASDM) with space weather inputs.
