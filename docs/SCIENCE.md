# POLARIS Scientific Methodology

## 1. Radar Polarimetric Ice Detection

### 1.1 Why Radar Sees Through Regolith

Optical sensors (cameras, spectrometers) can only observe the lunar surface. In Permanently Shadowed Regions (PSRs), there is no direct sunlight — cameras cannot image these areas at all.

**Microwave radar penetrates the surface.** The Chandrayaan-2 DFSAR operates at:
- **L-band (1.25 GHz, λ = 24 cm):** Penetrates ~5 m into dry regolith
- **S-band (2.5 GHz, λ = 12 cm):** Penetrates ~1-2 m into dry regolith

This makes DFSAR the ideal instrument for detecting **subsurface** ice deposits that optical instruments cannot see.

### 1.2 The Ambiguity Problem

Both subsurface ice AND rough rocky surfaces produce high CPR (Circular Polarization Ratio) values. This is because:

- **Rocky terrain:** Radar bounces off angular boulders at the surface (double-bounce scattering) → CPR > 1 but polarization is maintained → **high DOP**
- **Subsurface ice:** Radar penetrates regolith, scatters from multiple ice grain boundaries (volumetric scattering) → CPR > 1 AND polarization is destroyed → **low DOP**

### 1.3 Triple-Gate Solution

Our pipeline exploits this physical difference using three independent tests:

```
Gate 1: CPR > 1.0   → Confirms anomalous scattering (could be ice OR rocks)
Gate 2: DOP < 0.13  → Confirms depolarisation (rules out surface rocks)
Gate 3: H > 0.7     → Confirms random volumetric scattering (confirms ice)
```

All three must pass simultaneously. This is physically rigorous because:
- Gate 1 is necessary but not sufficient
- Gate 2 eliminates the dominant source of false positives (rocky terrain)
- Gate 3 provides an independent confirmation via information-theoretic entropy

---

## 2. Doubly Shadowed Craters (DSCs)

### What Makes Them Special

A PSR receives no direct sunlight. But it may still receive **reflected thermal radiation** from nearby sunlit crater walls, raising its temperature above the ice stability threshold (~110 K for water ice).

A **doubly shadowed crater** is a small crater *inside* a PSR that is also shielded from reflected radiation by its own walls. This creates temperatures of **~25 K (−248°C)** — cold enough to preserve ice for billions of years.

### How We Identify Them

1. Compute annual illumination fraction for every DEM pixel using SPICE solar angles
2. Pixels with illumination < 0.01% → PSR
3. Within PSRs, find local topographic depressions (crater-like features)
4. These depressions that are also thermally isolated from surrounding walls → DSCs

---

## 3. Bayesian Multi-Stream Fusion

### Independent Evidence Streams

| Stream | Measurement | Ice Signature |
|--------|-------------|---------------|
| Radar (CPR+DOP+H) | Polarimetric scattering | High CPR, low DOP, high entropy |
| Morphology (OHRC) | Crater rim shape | Lobate rims (ice-impact interaction) |
| Thermal (Diviner/SPICE) | Surface temperature | < 110 K annual maximum |
| Illumination (DEM) | Shadow duration | PSR or DSC classification |

### Fusion Formula

```
P(ice | all_evidence) ∝ P(radar | ice) × P(morphology | ice) × P(thermal | ice) × P(prior)
```

Where:
- `P(radar | ice)` = sigmoid function of triple-gate score
- `P(morphology | ice)` = lobate rim probability from OHRC morphological analysis
- `P(thermal | ice)` = probability derived from annual max temperature vs. ice stability curve
- `P(prior)` = 0.056, calibrated from LCROSS Cabeus crater ground truth (5.6% water by weight)

The posterior is computed per pixel, producing a continuous probability map (0.0 to 1.0).

---

## 4. ISRU Readiness Score (IRS)

The IRS is our novel composite metric that converts scientific detections into a mission-actionable decision number:

```
IRS = 0.40 × P(ice) + 0.25 × TerrainSafety + 0.20 × SolarAvailability + 0.15 × Proximity
```

| Component | Calculation | Range |
|-----------|------------|-------|
| P(ice) | Bayesian posterior probability at site centre (3×3 pixel mean) | 0–1 |
| TerrainSafety | 1 − (slope/slope_max)², penalised for boulders | 0–1 |
| SolarAvailability | Fraction of lunar day with direct solar illumination at site | 0–1 |
| Proximity | 1 / (1 + distance_to_DSC_km) | 0–1 |

The IRS score is normalised to **0–100**. A score ≥ 70 indicates a viable mission target.

---

## 5. Monte Carlo Volumetric Estimation

We do NOT report a single ice volume number. We report a **probabilistic range** using Monte Carlo simulation:

1. Draw 10,000 samples from parameter distributions:
   - ε_ice ~ Normal(3.15, 0.05)
   - ε_regolith ~ Uniform(2.7, 3.0)
   - depth ~ Normal(5.0, 1.5) metres

2. For each sample, compute ice volume fraction via Lorentz-Lorenz mixing:
   - f_ice = (ε_measured − ε_regolith) / (ε_ice − ε_regolith)

3. Compute mass = f_ice × area × depth × 917 kg/m³

4. Report P10 (conservative), P50 (median), P90 (optimistic) estimates

This approach is borrowed from petroleum reservoir engineering and provides decision-makers with a quantified range of uncertainty — not a false precision single number.
