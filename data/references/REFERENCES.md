# References & Research Papers

## Core References Used in POLARIS

### Primary (Peer-Reviewed Publications)

1. **Bhatt, M., et al. (2024).** "Evidence of subsurface water-ice in doubly shadowed craters of the lunar south polar region from Chandrayaan-2 DFSAR data." *Icarus*, 408, 115825.
   - **DOI:** https://doi.org/10.1016/j.icarus.2023.115825
   - **Relevance:** Defines the CPR > 1 AND DOP < 0.13 dual criterion used as the foundation of our detection pipeline. Identifies 4 doubly shadowed craters in Faustini with ice signatures.

2. **Colaprete, A., et al. (2010).** "Detection of Water in the LCROSS Ejecta Plume." *Science*, 330(6003), 463–468.
   - **DOI:** https://doi.org/10.1126/science.1186986
   - **Relevance:** Provides the only confirmed in-situ measurement of lunar polar water (5.6 wt% in Cabeus). Used as our Bayesian prior calibration point (P(ice) = 0.056).

3. **Spudis, P. D., et al. (2013).** "Evidence for Water Ice on the Moon: Results for Anomalous Polar Craters from the LRO Mini-RF Imaging Radar." *JGR Planets*, 118(10), 2016–2029.
   - **DOI:** https://doi.org/10.1002/jgre.20156
   - **Relevance:** Baseline CPR methodology from Mini-RF; our pipeline improves upon this with full-polarimetric DFSAR and DOP analysis.

4. **Rubanenko, L., et al. (2019).** "Thick ice deposits in shallow simple craters on the Moon and Mercury." *Nature Geoscience*, 12, 597–601.
   - **DOI:** https://doi.org/10.1038/s41561-019-0405-8
   - **Relevance:** Demonstrates subsurface ice is 5–8× more abundant than surface ice. Justifies our focus on subsurface detection via L-band radar.

5. **Hayne, P. O., et al. (2021).** "New constraints on thermal and dielectric properties of lunar regolith from LRO Diviner and Mini-RF observations." *JGR Planets*, 126, e2020JE006392.
   - **DOI:** https://doi.org/10.1029/2020JE006392
   - **Relevance:** Temperature maps constraining ice stability depths; used in Module 3 thermal stability calculation.

6. **Li, S., et al. (2018).** "Direct evidence of surface exposed water ice in the lunar polar regions." *PNAS*, 115(36), 8907–8912.
   - **DOI:** https://doi.org/10.1073/pnas.1802345115
   - **Relevance:** M³ spectroscopic confirmation of surface ice; cross-validates our radar-based detections.

7. **Mitrofanov, I. G., et al. (2010).** "Hydrogen Mapping of the Lunar South Pole Using the LRO Neutron Detector Experiment LEND." *Science*, 330(6003), 483–486.
   - **DOI:** https://doi.org/10.1126/science.1185696
   - **Relevance:** Neutron spectrometer H-enrichment maps; used for validation overlay.

### ISRO Official Documents

8. **ISRO Annual Report 2023-24** — Chandrayaan-2 Science Results Section
   - **URL:** https://www.isro.gov.in/AnnualReport.html
   - **Relevance:** Official documentation of DFSAR and OHRC instrument performance and science objectives.

9. **ISSDC Chandrayaan-2 Data Policy**
   - **URL:** https://pradan.issdc.gov.in
   - **Relevance:** Data access terms, acknowledgment requirements.

### NASA/International Reports

10. **Artemis III Science Definition Team Report (2024)**
    - **URL:** https://www.nasa.gov/artemis-science
    - **Relevance:** Lists south pole candidate landing sites; aligns with ISRO/POLARIS target zones.

11. **Bickel, V. T., et al. (2020).** "Automated detection of lunar rockfalls using a convolutional neural network." *IEEE TGRS*, 58(7), 4876–4885.
    - **DOI:** https://doi.org/10.1109/TGRS.2020.2968993
    - **Relevance:** Provides the labelled boulder dataset used for U-Net training; methodology adapted for OHRC.

12. **Deutsch, A. N., et al. (2022).** "Surface Roughness of Permanently Shadowed Regions Using Mini-RF." *Icarus*, 378, 114935.
    - **DOI:** https://doi.org/10.1016/j.icarus.2022.114935
    - **Relevance:** Distinguishing roughness from ice using CPR — our DOP gate addresses this limitation.
