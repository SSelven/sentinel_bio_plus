
## Atmospheric Gases in Space & Their Role in CubeSat Drag During Solar Activity

In low Earth orbit (LEO, ~200–1000 km), "space" is not a perfect vacuum — it contains a very thin neutral thermosphere/exosphere made up of several atomic and molecular species. These gases cause **atmospheric drag**, the dominant force that gradually lowers a CubeSat's orbit until re-entry.

The **composition changes dramatically with altitude**, and **solar activity** (especially during solar maximum) strongly controls both the **total density** and the **relative abundance** of each species.

### Main Neutral Species in the Thermosphere (100–700 km)

| Species       | Chemical Symbol | Mass (u) | Dominant Altitude Range | Behavior During Solar Maximum (High F₁₀.₇)                     | Drag Impact on CubeSat                                      |
|---------------|------------------|----------|---------------------------|----------------------------------------------------------------|-------------------------------------------------------------|
| **Molecular Nitrogen** | N₂              | 28       | 100–200 km (peaks ~120–150 km) | Density increases moderately; still dominant below ~200 km     | Main drag contributor below ~250 km                         |
| **Molecular Oxygen**   | O₂              | 32       | 100–250 km (peaks ~150–180 km) | Strong increase; dissociates more into atomic oxygen           | Significant below ~300 km; decreases relative importance higher up |
| **Atomic Oxygen**      | O               | 16       | 200–700+ km (dominant above ~250 km) | **Very large increase** — becomes overwhelmingly dominant      | **Primary drag species** at typical CubeSat altitudes (400–600 km) |
| **Atomic Helium**      | He              | 4        | Above ~500–600 km (dominant >700 km) | Increases significantly; lightest species, escapes more easily | Minor at 400–600 km; becomes important >700 km              |
| **Atomic Hydrogen**    | H               | 1        | Above ~600–800 km (exosphere) | Increases; very light, high thermal velocity                   | Negligible for drag below ~800 km                           |
| **Argon**              | Ar              | 40       | 100–300 km (trace)        | Moderate increase; heavier → settles lower                     | Very minor contribution                                     |

### How Solar Activity Changes the Picture

During **solar maximum** (high F₁₀.₇ ≈ 200–300 sfu):

1. **Total neutral density increases dramatically**  
   - At 500 km: density can be **5–10× higher** than during solar minimum  
   → Direct linear increase in drag force (F_d ∝ ρ)

2. **Composition shifts upward**  
   - Stronger EUV heating → higher thermospheric temperature → larger scale height  
   - Molecular species (N₂, O₂) dissociate more efficiently into atomic oxygen  
   - **Atomic oxygen (O) becomes even more dominant** at CubeSat altitudes (400–600 km)  
   → O is lighter → higher thermal velocity → more momentum transfer per collision → slightly higher effective drag coefficient

3. **Scale height increases**  
   - H ≈ kT / (m g) → higher T → larger H → density falls off more slowly with altitude  
   → Satellites at a fixed altitude experience **much higher density** during solar max

### Summary Table: Solar Activity Effect on Drag-Relevant Species

| Altitude | Dominant Gas (Solar Min) | Dominant Gas (Solar Max) | Density Increase Factor (typical) | Main Reason for Higher Drag                     |
|----------|---------------------------|---------------------------|------------------------------------|--------------------------------------------------|
| 200–300 km | N₂, O₂                   | N₂, O₂ → more O           | ~3–6×                              | Higher total density + partial dissociation      |
| 400–500 km | O (already dominant)     | O (even more dominant)    | ~5–10×                             | Large total density increase + O dominance       |
| 500–600 km | O                        | O                         | ~6–15×                             | Very strong density amplification                |
| >700 km  | O → He                   | He becomes more prominent | ~5–20×                             | Extreme expansion; lighter species dominate      |

### Practical Implications for CubeSats

- At typical CubeSat altitudes (**400–600 km**), **atomic oxygen (O)** is responsible for **~80–95% of drag during both solar min and max**.
- Solar maximum does **not** change the dominant species — it massively **amplifies** the density of atomic oxygen.
- Because drag force scales **linearly** with density (F_d = ½ ρ v² C_d A), a **5–10× density increase** → **5–10× higher drag** → **much shorter orbital lifetime**.
- Your Monte Carlo simulation already captures this via the **F₁₀.₇-dependent density scaling** (k_sol(h) exponent) calibrated to NRLMSISE-00 behavior.

**Bottom line**  
When solar activity increases (higher F₁₀.₇), the thermosphere doesn't just get denser — it **expands upward** and **shifts toward lighter atomic species** (especially atomic oxygen). For CubeSats in the 400–600 km range, this means **atomic oxygen density can increase by a factor of 5–15×**, which is why your solar-max vs solar-min lifetime ratios are often ~4–10×.

This is the single biggest reason why launching during **solar minimum** can give a CubeSat **several times longer life** than launching during **solar maximum**.

Further reading:
- Picone et al. (2002) – NRLMSISE-00 model (species profiles & solar response)
- Emmert et al. (various) – thermospheric composition changes over solar cycles
- Doornbos (2012) – satellite drag & thermospheric species effects
