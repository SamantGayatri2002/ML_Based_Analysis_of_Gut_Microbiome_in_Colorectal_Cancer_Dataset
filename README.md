# ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset


This repository documents my learning journey and practice in **shotgun metagenomics data analysis**, focusing on how raw sequencing reads can be processed, analyzed, and interpreted to study microbial communities along with the integration of Machine Learning pipeline.




## 🧬 Shotgun Metagenomics:
(From Raw Fastq reads to Microbial features associated with Colorectal Cancer Using Machine Learning.)


----

### **🎯 Introduction**

Shotgun metagenomics allows sequencing of all microbial DNA present in a sample, providing species-level resolution of microbial communities.

This repository contains a complete workflow combined with machine learning to:

- Profile microbial communities  
- Quantify species-level abundances  
- Build ML models to effectively classify the Shotgun metagenomic samples 
- Identify the most important microbial signatures  

This project includes scripts, metadata, taxonomic profiling output, necessary referneces.

---

## 📂 **Repository Structure**

```
**ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset**/
│
├── **Meta_Results/** # QC outputs, Kraken2 files, Bracken abundance files
│
├── **Obtaining_and_Preparing_the_metadata/** # SRA metadata, phenotype data, metadata cleaning steps
│
├── **Project_Workflow/** # Step-by-step bioinformatics pipeline scripts
│
├── **Machine_Learning_(Advanced_Dataset)/** # Additional jupitor notebooks, For advanced dataset
│
├── **ML_analysis.ipynb** # Main ML workflow
|
├── **combine_bracken_outputu.py **# Script to merge species abundance tables
|
├── **combined_otu_data.tsv** # Final OTU/species abundance table
|
└── **README.md** # Project documentation

```

---
## 📊 Workflow Diagram


<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/4956ffdc-2676-4cf4-b740-b83f10839faf" />


# 🚀 **Workflow Summary**

### 🔄 **1. Raw Data → Cleaned Reads**
- Download SRA runs  
- Perform QC  
- Remove adapters  
- Remove human reads (host decontamination)

### 🧬 **2. Species-Level Profiling**
- Run **Kraken2** for classification  
- Run **Bracken** for abundance estimation  
- Merge all Bracken outputs into a master abundance table  

### 📊 **3. Metadata Integration**
- Link species abundances with CRC metadata  
- Prepare ML-ready dataset  

### 🤖 **4. Machine Learning**
- Preprocessing & normalization  
- Feature selection  
- Model training (Logistic Regression, Random forest, SVM etc.)  
- Performance evaluation   

---

### 🔗 Direct Folder/File Links

- **Meta_Results**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/tree/main/Meta_Results  

- **Obtaining_and_Preparing_the_metadata**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/tree/main/Obtaining_and_Preparing_the_metadata  

- **Project_Workflow**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/tree/main/Project_Workflow  

- **Machine_Learning_(Advanced_Dataset)**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/tree/main/Machine_Learning_(Advanced_Dataset)  

- **combine_bracken_outputu.py**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/blob/main/combine_bracken_outputu.py  

- **combined_otu_data.tsv**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/blob/main/combined_otu_data.tsv  

- **ML_analysis.ipynb**  
  https://github.com/SamantGayatri2002/ML_Based_Analysis_of_Gut_Microbiome_in_Colorectal_Cancer_Dataset/blob/main/ML_analysis.ipynb  

---

## 🧰 Tools used

These badges represent the primary tools used:

### 🔧 **Bioinformatics Tools**
![Kraken2](https://img.shields.io/badge/Kraken2-Metagenomics-blue)
![Bracken](https://img.shields.io/badge/Bracken-Abundance%20Estimation-green)
![FastQC](https://img.shields.io/badge/FastQC-Quality%20Control-orange)
![Bowtie2](https://img.shields.io/badge/Bowtie2-Host%20Decontamination-yellow)

### 🤖 **Machine Learning Tools and Libraries**
![Python](https://img.shields.io/badge/Python-3.10+-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML%20Models-orange)
![NumPy](https://img.shields.io/badge/NumPy-Data%20Handling-green)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Frame%20Processing-yellow)

---

## 🎯 Why Shotgun Metagenomics (vs 16S rRNA)  

- Enables species-level (or even strain-level) resolution.  
- Captures entire genomic content: bacteria, archaea, viruses, fungi — not limited to marker genes.  
- Allows assessment of functional potential (genes, pathways), beyond taxonomic profiling.

---

## 🚀 How to Use / Reproduce  

1. Clone this repository  
2. Acquire raw sequencing data (or metadata + reads) — as per your data source  
3. Run preprocessing pipeline in `Project_Workflow/` to generate species abundance tables  
4. Use `combine_bracken_outputu.py` to aggregate abundances  
5. Run `ML_analysis.ipynb` to perform ML modelling and analyses  
6. Review / analyze output performance, important taxa / signatures, etc.  

*(Modify paths, parameters as needed depending on your dataset and resources.)*

---

## 🧩 Possible Future Directions  

- Automate the full pipeline using a workflow manager (e.g. Snakemake or Nextflow)  
- Expand to functional profiling (gene/pathway-level) using tools like HUMAnN3 or eggNOG‑mapper  
- Apply to larger cohorts, cross-study validations or external datasets  
- Explore clustering, diversity analysis, longitudinal data (if available)  
- Add feature interpretation, biomarker discovery, and robustness analyses  

---

## 📄 License & Acknowledgments  

This project was part of the online workshop **“Machine Learning in NGS Data Analysis”** under guidance of *Bablu Kumar (BCD Analytics Hub)* — many thanks to the organizers for introducing me to computational pipelines for microbiome research.  

---

## 📝 Contact 

For queries or collaboration:

Author: Gayatri Sunil Samant<br>
GitHub: https://github.com/SamantGayatri2002
