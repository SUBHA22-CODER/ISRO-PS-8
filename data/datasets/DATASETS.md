# Datasets for POLARIS — Download Instructions

> All datasets listed here are **free and open-access**. Total acquisition cost: **₹0**.

---

## 1. ISRO Chandrayaan-2 Data (Primary — Provided by ISRO for Hackathon)

### DFSAR (Dual Frequency SAR) — L-band & S-band SLC
- **Portal:** [ISRO PRADAN](https://pradan.issdc.gov.in)
- **Registration:** Free (email verification required)
- **Browser:** Use Chrome; session times out after 30 min
- **Navigation:** Login → Chandrayaan-2 → DFSAR → Select south polar passes
- **Format:** HDF5 or ENVI binary (complex SLC data in HH, HV, VH, VV polarizations)
- **Data ID pattern:** `ch2_sar_nci_*` (Level-1 SLC products)

> **Hackathon Note:** Participants are supplied with DFSAR data of a doubly shadowed crater in the south polar region. Check the hackathon data package first.

### OHRC (Orbiter High Resolution Camera) — 0.25 m imagery
- **Portal:** [ISRO Map Browser](https://chmapbrowse.issdc.gov.in)
- **Registration:** Same PRADAN account
- **Navigation:** Login → Select instrument `CH2_OHR_Calibrated_Product` → Navigate to south pole
- **Format:** PDS4 .IMG files
- **Processing:** Use `pds4_tools` Python library or GDAL with PDS4 driver

### TMC-2 DEM — 5 m terrain model
- **Portal:** [ISRO PRADAN](https://pradan.issdc.gov.in)
- **Product:** `CH2_TMC_DEM` under Chandrayaan-2 products
- **Format:** GeoTIFF

---

## 2. NASA Lunar Data (Supplementary — Free)

### LOLA DEM (Lunar Orbiter Laser Altimeter)
- **Best product:** Gridded Data Records (GDR) — 5 m to 60 m resolution
- **Download:** [PDS Geosciences Node — LRO LOLA](https://pds-geosciences.wustl.edu/missions/lro/lola.htm)
- **South Pole GeoTIFFs:** [NASA PGDA — LOLA South Pole COGs](https://pgda.gsfc.nasa.gov/products/78)
- **Search tool:** [Lunar Orbital Data Explorer (ODE)](https://ode.rsl.wustl.edu/moon/)

### SLDEM2015 (LOLA + Kaguya Merged)
- **Resolution:** ~60 m global, ~5 m polar
- **Download:** [PDS Geosciences — SLDEM2015](https://pds-geosciences.wustl.edu/missions/lro/lola.htm)

### Mini-RF CPR Maps (for cross-validation)
- **Instrument:** LRO Mini-RF (hybrid-pol SAR, S-band and X-band)
- **Download:** [PDS Lunar ODE — Mini-RF Derived Data](https://ode.rsl.wustl.edu/moon/)
- **Product type:** `MAPCDR` (Map-projected Calibrated Data Record) includes CPR

### Diviner Surface Temperature
- **Instrument:** LRO Diviner Lunar Radiometer
- **Download:** [PDS Geosciences — Diviner](https://pds-geosciences.wustl.edu/missions/lro/diviner.htm)
- **Key product:** Polar temperature maps (annual max/min)
- **Resolution:** ~200 m

### LRO NAC Imagery (for U-Net training)
- **Resolution:** 0.5 m/pixel
- **Search:** [LROC Image Search Tool](https://wms.lroc.asu.edu/lroc/search)
- **Use:** Training boulder detection model (transfer learning from OHRC)

---

## 3. SPICE Kernels (Illumination Modelling)

### NASA NAIF Generic Kernels
- **Download:** [NAIF Generic Kernels](https://naif.jpl.nasa.gov/naif/data_generic.html)
- **Required:** LSK (leap seconds), PCK (planetary constants), SPK (solar system ephemeris)
- **Install:** `pip install spiceypy`

### LRO SPICE Archive
- **Download:** [PDS LRO SPICE Dataset](https://naif.jpl.nasa.gov/naif/data_archived.html)

### Chandrayaan-1 SPICE (via ESA)
- **Download:** [ESA SPICE Service](https://doi.org/10.5270/esa-mfpmcbt)

---

## 4. Boulder Detection Training Data

### Bickel et al. (2020) — Labeled Lunar Boulders
- **Description:** 30,000+ hand-labelled boulders from LRO NAC imagery
- **Download:** [Zenodo — Boulder Dataset](https://zenodo.org/records/3967885)
- **Format:** Image patches + segmentation masks
- **Use:** Train/fine-tune U-Net for boulder detection on OHRC

### Alternative: Otsu Thresholding (No Training Data Needed)
- If boulder labels are unavailable, the pipeline falls back to Otsu binary thresholding
- This is the default for the hackathon demo

---

## 5. Reference / Calibration Data

### LCROSS Impact Results (Ground Truth)
- **Paper:** Colaprete et al. (2010), *Science*, 330(6003)
- **Key value:** 5.6 ± 2.9 wt% water in Cabeus crater ejecta
- **Use:** Calibrate Bayesian prior P(ice) = 0.056

### Bhatt et al. (2024) DFSAR Results
- **Paper:** "Evidence of subsurface water-ice in doubly shadowed craters...", *Icarus*
- **Key values:** CPR > 1 AND DOP < 0.13 in 4 DSCs inside Faustini
- **Use:** Validate our CPR/DOP computation pipeline

---

## Download Script

```bash
# Install required tools
pip install spiceypy gdown

# Download LOLA South Pole DEM (public, no login)
wget https://pgda.gsfc.nasa.gov/data/LOLA_SP_DEM/lola_sp_dem_5m.tif -O data/datasets/lola_sp_dem.tif

# Download Bickel et al. boulder labels from Zenodo
wget https://zenodo.org/records/3967885/files/boulders_dataset.zip -O data/datasets/boulders.zip
unzip data/datasets/boulders.zip -d data/datasets/boulders/

# Download SPICE generic kernels
python -c "
import spiceypy as spice
# Generic kernels are loaded at runtime; download LSK from NAIF
import urllib.request
urllib.request.urlretrieve(
    'https://naif.jpl.nasa.gov/pub/naif/generic_kernels/lsk/naif0012.tls',
    'data/datasets/naif0012.tls'
)
print('SPICE LSK kernel downloaded.')
"

# ISRO PRADAN data requires manual login at https://pradan.issdc.gov.in
echo "For DFSAR and OHRC data, log in to https://pradan.issdc.gov.in"
```

---

## Data Directory Structure After Download

```
data/
├── datasets/
│   ├── dfsar/                    # DFSAR SLC data (from PRADAN)
│   │   ├── l_band_slc.h5        # L-band HH/HV/VH/VV
│   │   └── s_band_slc.h5        # S-band HH/HV/VH/VV
│   ├── ohrc/                     # OHRC imagery (from PRADAN)
│   │   └── ohrc_south_pole.img
│   ├── dem/                      # Digital Elevation Models
│   │   ├── lola_sp_dem.tif       # NASA LOLA 5m
│   │   └── tmc2_dem.tif          # ISRO TMC-2 5m
│   ├── spice/                    # SPICE kernels
│   │   └── naif0012.tls
│   ├── diviner/                  # Thermal data
│   │   └── diviner_sp_tmax.tif
│   ├── minirf/                   # Mini-RF CPR (cross-validation)
│   │   └── minirf_cpr_sp.img
│   └── boulders/                 # U-Net training labels
│       ├── images/
│       └── masks/
└── DATASETS.md                   ← This file
```
