## ⚙️ Tools and Databases used during the entire pipeline

### 1️⃣ Data Acquisition (SRA Data)

* **Why**: Public metagenomic datasets are often deposited in repositories like NCBI SRA (Sequence Read Archive). We fetch them to reproduce studies or test pipelines.
* **How it works**: SRA files are downloaded and converted into FASTQ format for downstream analysis.
* **Alternatives**:

  * [ENA (European Nucleotide Archive)](https://www.ebi.ac.uk/ena/browser/home)
  * [MG-RAST](https://www.mg-rast.org/) for public metagenome datasets.

---

### 2️⃣ Quality Control (FastQC + Fastp)

* **FastQC**

  * **Why**: To assess sequencing quality (per-base quality, GC content, adapter contamination).
  * **How**: Generates a visual report to highlight issues.
  * **Alternatives**: MultiQC (for summarizing multiple samples).

* **Fastp**

  * **Why**: Performs **trimming, filtering, and adapter removal** to improve data quality before downstream analysis.
  * **How**: Removes low-quality reads, trims adapters, and generates QC reports.
  * **Alternatives**:

    * Trimmomatic (flexible but slower).
    * Cutadapt (adapter-focused).
    * BBDuk (fast and memory-efficient).

---

### 3️⃣ Host Read Removal (Bowtie2)

* **Why**: In human microbiome studies, a large fraction of reads may come from the host (human DNA). Removing them ensures only microbial reads are analyzed.
* **How**: Aligns reads to the human reference genome (GRCh38). Reads mapping to the host are discarded.
* **Alternatives**:

  * BWA-MEM (another aligner).
  * Kraken2 with human reference for classification-based filtering.
  * HISAT2 (faster but optimized for transcriptomics).

---

### 4️⃣ Taxonomic Classification (Kraken2)

* **Why**: Assign reads to microbial taxa to understand community composition.
* **How**: Uses a k-mer based exact matching algorithm against a database (e.g., **UHGG v2.0.2 for gut microbiome**).
* **Advantages**: Very fast, scalable, works well on large datasets.
* **Alternatives**:

  * Kaiju (protein-level classification, better for viruses).
  * MetaPhlAn (marker-gene based, more accurate at species-level, but slower).
  * Centrifuge (memory-efficient for large databases).

---

### 5️⃣ Abundance Estimation (Bracken)

* **Why**: Kraken2 sometimes overestimates species due to shared k-mers. Bracken refines abundance estimates.
* **How**: Uses Bayesian re-estimation to improve species/strain-level abundance accuracy.
* **Alternatives**:

  * MetaPhlAn2/3 (direct abundance estimation).
  * mOTUs profiler (strain-level abundance).

---

### 6️⃣ Downstream Analysis & Machine Learning

* **Why**: After generating abundance tables, we can use machine learning to:

  * Predict disease states,
  * Identify biomarkers,
  * Explore microbiome-environment associations.
* **How**: Features = microbial abundances; Labels = phenotype (e.g., disease vs. healthy).
* **Common Models**:

  * Random Forest (robust, interpretable feature importance).
  * SVM (good for high-dimensional data).
  * KNN , Logistic Regression , XGBoost.
* **Alternatives**: Statistical methods like LEfSe, DESeq2 for differential abundance.

---

## 📦 Database Requirements

* **Kraken2 Database**: Unified Human Gastrointestinal Genome (UHGG v2.0.2)

  * Optimized for gut microbiome studies.
  * For 8 GB RAM systems: [Download link](https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08_GB_20250714.tar.gz)

* **Reference Genome for Host Removal**: Human GRCh38 no-alt analysis set indexed for Bowtie2.

📄 **Reference**:
Almeida A., Mitchell A.L., Boland M. *et al.*
**A unified catalog of 204,938 reference genomes from the human gut microbiome**. *Nature Biotechnology* (2021).
