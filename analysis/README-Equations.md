
## Key Equations Used in the Simulation

This Monte Carlo simulator estimates 3U CubeSat orbital lifetime using simplified but physically motivated models. Below are the principal equations implemented in the code.

### 1. Orbital Decay Rate (Circular Orbit Assumption)

The core rate of change of semi-major axis **a** due to atmospheric drag (averaged over one orbit):

$$
\frac{da}{dt} = - \rho \cdot C_d \cdot \frac{A}{m} \cdot v \cdot a
$$

where:
- ρ        = local atmospheric mass density (kg/m³)
- C_d      = drag coefficient (default = 2.2)
- A        = cross-sectional area (m²)
- m        = satellite mass (kg)
- v        = orbital velocity ≈ √(μ / a)  (m/s)
- a        = semi-major axis (m)
- μ        = Earth gravitational parameter = G · M_Earth ≈ 3.986004418 × 10¹⁴ m³/s²

**Implemented in:** `orbital_decay_ode()`

### 2. Gravitational Harmonics Corrections (J₂, J₃, J₄)

Multiplicative corrections applied to da/dt to account for oblate atmosphere and higher-order zonal effects (EGM96 model):

- J₂ correction (dominant oblateness term)

$$
f_{J_2} = 1 + \frac{3}{2} J_2 \left( \frac{R_E}{a} \right)^2 \left( 1 - \frac{3}{2} \sin^2 i \right)
$$

- J₃ correction (odd zonal – North-South asymmetry)

$$
f_{J_3} = 1 + \frac{J_3}{J_2} \left( \frac{R_E}{a} \right) \sin i \cdot 0.5
$$

- J₄ correction (higher-order even zonal)

$$
f_{J_4} = 1 + \frac{3}{8} \frac{J_4}{J_2} \left( \frac{R_E}{a} \right)^2 \left( 1 - 5 \sin^2 i \right)
$$

Final corrected decay rate:

$$
\frac{da}{dt} \leftarrow \frac{da}{dt} \times f_{J_2} \times f_{J_3} \times f_{J_4}
$$

**Constants used** (EGM96):  
J₂ = 1.08263 × 10⁻³  
J₃ = –2.54 × 10⁻⁶  
J₄ = –1.62 × 10⁻⁶  

**Implemented in:** `orbital_decay_ode()`

### 3. Atmospheric Density Model

$$
\rho(h, F_{10.7}, i, Ap) = \rho_{\text{ref}}(h) \times f_{\text{solar}} \times f_{\text{geomag}} \times f_{\text{latitude}} \times f_{J_2}
$$

- Reference density ρ_ref(h) → interpolated from 1 km resolution NRLMSISE-00 calibrated table (100–700 km)
- Solar activity factor

$$
f_{\text{solar}} = \exp\left( k_{\text{sol}}(h) \cdot (F_{10.7} - 150) \right)
$$

$$
k_{\text{sol}}(h) = 0.008 + 0.012 \cdot \exp\left( -\frac{h - 300}{200} \right) \quad (\text{km}^{-1})
$$

- Geomagnetic activity factor

$$
f_{\text{geomag}} = \exp\left( k_{\text{Ap}}(h) \cdot (Ap - 4) \right)
$$

$$
k_{\text{Ap}}(h) = 0.002 + 0.009 \left( \frac{h}{600} \right)^{1.5} \quad (\text{km}^{-1})
$$

- Latitude / inclination correction (equatorial bulge + auroral heating)

$$
f_{\text{latitude}} = 1 + 0.06 \cos(2i) + 0.04 \sin^2 i
$$

**Implemented in:** `atmospheric_density()`

### 4. Orbital Velocity

Circular orbit approximation:

$$
v = \sqrt{\frac{\mu}{a}}
$$

### 5. Deorbit Condition

Simulation stops when altitude reaches:

$$
h_{\text{deorbit}} = 100\,\text{km} \quad \Rightarrow \quad a_{\text{deorbit}} = R_E + 100\,\text{km}
$$

**Implemented as event** in `solve_ivp`

### 6. Lifetime Conversion

Lifetime in years:

$$
\tau = \frac{t_{\text{deorbit}}}{365.25 \times 86400}
$$

where t_deorbit is the time when the event is triggered (seconds).

### 7. Ballistic Coefficient (reference value)

$$
BC = \frac{m}{C_d \cdot A}
$$

Used only for reporting / interpretation (not directly in ODE).

**Default values in code**  
m = 3.5 kg  
A = 0.04 m²  
C_d = 2.2  
→ BC ≈ 39.8 kg/m²

### Summary – Order of Magnitude

At 525 km, moderate conditions (F₁₀.₇ ≈ 150, Ap ≈ 4):

- ρ ≈ 3–5 × 10⁻¹³ kg/m³
- v ≈ 7.6 km/s
- |da/dt| ≈ few meters per day → lifetime ~ few to ~15 years depending on solar cycle phase

All equations are implemented in the function `orbital_decay_ode()` with the density corrections applied in `atmospheric_density()`.

For references to derivation and validation see the main references section.
