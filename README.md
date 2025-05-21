# 🌳 OpenAtlas – AGB Estimation from Canopy Height

As part of OpenAtlas's expansion into emissions and biomass monitoring, we’ve developed a new **Above Ground Biomass Index (AGB)** based on our existing **Global Canopy Height Model (GCHM)**. This repository contains two methods implemented in Google Colab notebooks, benchmarked on multiple AOIs.

---

## 🧠 Scientific Background

We built on the approach proposed in **AGBD (Sialelli et al., 2024)** — one of the most accurate global AGB datasets — which uses GEDI footprints, Sentinel-2 and PALSAR-2 data to train CNN-based models.

However, while AGBD uses machine learning to estimate biomass coefficients `(a, b)`, our method avoids this source of bias by relying on **field-validated allometric equations** from the literature.

AGB is computed using: AGB = a × H^b

Where `H` is the canopy height (from GCHM), and `(a, b)` depend on the land cover or biome.

---

## 🗂️ Land Cover–Based Coefficients (LC100)

| LC Code | Class Name                           | Description                             | a      | b    | Reference                      |
|---------|---------------------------------------|-----------------------------------------|--------|------|-------------------------------|
| 10      | Broadleaved evergreen trees           | Tropical humid forest                   | 0.067  | 2.52 | Feldpausch et al., 2012       |
| 20      | Broadleaved deciduous trees           | Tropical dry/temperate forest           | 0.045  | 2.40 | Saatchi et al., 2011          |
| 30      | Needleleaved evergreen trees          | Boreal/temperate coniferous forest      | 0.048  | 2.38 | Santoro et al., 2011          |
| 40      | Needleleaved deciduous trees          | Boreal forest (rare)                    | 0.045  | 2.30 | Santoro et al., 2011          |
| 50      | Mixed leaf trees                      | Mixed forest (temperate/tropical)       | 0.050  | 2.40 | Saatchi et al., 2011          |
| 60      | Mosaic tree/shrub >50%                | Wooded savanna, open forest             | 0.035  | 2.30 | Chave et al., 2005            |
| 70      | Mosaic herbaceous >50%                | Sparse tree savanna                     | 0.028  | 2.10 | Chave et al., 2005            |
| 80      | Shrubland                             | Dryland, woody bushes                   | 0.020  | 2.00 | (estimated from Saatchi)      |
| 160     | Herbaceous wetland                    | Seasonally flooded grasslands           | 0.030  | 2.30 | Based on Chave et al., 2005   |
| 170     | Mangroves                             | Tropical coastal swamp forest           | 0.110  | 2.20 | Simard et al., 2019           |

---

## ✅ Our Contributions

- We reuse AGBD’s land cover classification (Copernicus LC100)
- Instead of learning `(a, b)` through deep learning, we **assign them using literature-derived allometric models**
- This makes our model **physically consistent, interpretable**, and free of unnecessary learning bias
- The GCHM provides global, dynamic, high-resolution height inputs

---

## 🔀 Two Methods Compared

We implemented and tested two biomass estimation methods:

### 1. **RESOLVE Ecoregions**  
Each biome is assigned a unique (a, b) coefficient pair.  
✅ Low processing cost  
✅ Stable results  
✅ Coarse-scale, but fast

### 2. **LC100 Weighted Average**  
We compute a weighted average (a, b) over land cover types in the AOI.  
⚠️ More detailed but initially led to noisy, fragmented maps  
✅ Smoothed version improves interpretability  
✅ Useful for mixed-vegetation AOIs

> Both methods lead to very **similar AGB outputs**, confirming the robustness of our GCHM-based architecture.



![image](https://github.com/user-attachments/assets/da4cbb26-aafc-4331-9ea6-597e3e61269a)



![image](https://github.com/user-attachments/assets/4fb64fc6-74b0-4b0d-864d-e5a0f7f83d07)


---

## 💬 Why This Matters

Many competitors provide AGB maps but don’t compute canopy height — they often rely on AGBD directly.  
**Our strength:** we **separate structural estimation (GCHM)** from **biomass conversion**, enabling a more **modular, transparent, and accurate system**.

---


## 📁 Files (to be added)

- `method_1_collab.ipynb`: RESOLVE-based method
- `method_2_collab.ipynb`: LC100 weighted mean method
- `gchm_agb_utils.py`: Helper functions
- `example_outputs/`: GeoTIFF and visual maps
- `doc/`: Scientific references or report (optional)

---




