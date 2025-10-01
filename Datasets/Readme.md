# Reference Paper and Our Subset

## 📄 Reference Paper

**Title:**  
Pooled analysis of 3,741 stool metagenomes from 18 cohorts for cross-stage and strain-level reproducible microbial biomarkers of colorectal cancer  

**Authors:** Piccinno G., Thompson K.N., Manghi P., et al.  

**Journal:** *Nature Medicine*, 2025  

**DOI:** [10.1038/s41591-025-03693-9](https://doi.org/10.1038/s41591-025-03693-9)  

**Supplementary Metadata (Excel):**  
[MOESM2_ESM.xlsx](https://static-content.springer.com/esm/art%3A10.1038%2Fs41591-025-03693-9/MediaObjects/41591_2025_3693_MOESM2_ESM.xlsx)  

**Zenodo Dataset (profiles + metadata):**  
[10.5281/zenodo.15069069](https://zenodo.org/records/15069069)  

---

## 🔬 Study Overview

This landmark meta-analysis pooled **3,741 stool metagenomes** from **18 cohorts** (12 public datasets + 6 new ONCOBIOME cohorts) to identify reproducible **microbial biomarkers of colorectal cancer (CRC)** at the **species and strain level**.  

- Tools used: **MetaPhlAn4** for taxonomic profiling, **HUMAnN3.6** for functional profiling.  
- Cohorts spanned Asia, Europe, and North America.  
- The study provides openly available **metadata, profiles, and supplementary tables** for reproducibility.  

---

## 📊 Cohorts and BioProject IDs (as reported)

| Cohort / Study (paper) | BioProject / Accession | Reference |
|-------------------------|-------------------------|-----------|
| Feng 2015              | PRJEB7774              | [Feng et al.] |
| Gupta 2019             | PRJNA531273, PRJNA397112 | [Gupta et al.] |
| Thomas 2019            | PRJNA447983            | [Thomas et al.] |
| Vogtmann 2016          | PRJEB12449             | [Vogtmann et al.] |
| Wirbel 2019            | PRJEB27928             | [Wirbel et al.] |
| Yachida 2019           | DRA006684, DRA008156   | [Yachida et al.] |
| Yu 2017                | PRJEB10878             | [Yu et al.] |
| Zeller 2014            | PRJEB6070              | [Zeller et al.] |
| Liu 2022               | PRJNA731589            | [Liu et al.] |
| Yang 2020              | PRJNA429097            | [Yang et al.] |
| Yang 2021              | PRJNA763023            | [Yang et al.] |
| NHSII cohort (new)     | **PRJNA1237248**       | this study |
| Cohort 6 (new)         | PRJNA1167935           |      -      |
| ONCOBIOME CRC cohorts  | PRJEB72524, PRJEB72525, PRJEB72526, PRJEB72523 | ONCOBIOME |
| (plus other ONCOBIOME) | see supplementary file | - |

👉 Full metadata table with cohort/sample-level details: [Supplementary Excel](https://static-content.springer.com/esm/art%3A10.1038%2Fs41591-025-03693-9/MediaObjects/41591_2025_3693_MOESM2_ESM.xlsx)

---

## 📌 Our Project Focus

From the pooled dataset, **our analysis considered the cohort:**

| Cohort / Study | BioProject ID | Notes |
|----------------|---------------|-------|
| NHSII cohort   | **PRJNA1237248** | Selected for reproducibility testing and downstream modeling |

---

## 🚀 How to Use This Repository

1. **Download metadata & profiles**  
   - [Supplementary metadata (Excel)](https://static-content.springer.com/esm/art%3A10.1038%2Fs41591-025-03693-9/MediaObjects/41591_2025_3693_MOESM2_ESM.xlsx)  
   - [Zenodo dataset](https://zenodo.org/records/15069069)  

2. **Reproduce analysis steps**  
   - Preprocessing of raw reads  
   - Taxonomic profiling with MetaPhlAn4  
   - Functional profiling with HUMAnN3.6  
   - Statistical modeling (classification tasks for CRC vs controls)  

3. **Subset analysis**  
   - Our repo scripts focus on **PRJNA1237248** cohort.  
   - Code can be extended to other BioProjects listed above.  

---

## 📖 Citation

If you use this dataset or analysis in your own work, please cite:  

Piccinno G., Thompson K.N., Manghi P., et al. *Pooled analysis of 3,741 stool metagenomes from 18 cohorts for cross-stage and strain-level reproducible microbial biomarkers of colorectal cancer.* Nature Medicine (2025). DOI: [10.1038/s41591-025-03693-9](https://doi.org/10.1038/s41591-025-03693-9)  

And also cite this repository if you adapt workflows from here.  

---

## 🙌 Acknowledgements

- Original data generators across cohorts (see above references).  
- ONCOBIOME consortium for new CRC cohort sequencing.  
- Nature Medicine & Zenodo for making supplementary data accessible.  

