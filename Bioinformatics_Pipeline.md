# 🧑‍🎓 Shotgun Metagenomics — Processing Pipeline (Demo on ERR14218664)

As part of my **Shotgun Metagenomics Skill Development Workshop**, I am documenting the step-by-step processing of one sequencing run (**ERR14218664**).  

Since this is a learning exercise, I am running each step **individually** to understand the tools and their outputs clearly. Later, these steps can be combined into an automated script for larger datasets.

---

## 🔧 Pipeline Overview
This demo pipeline covers the following steps:

1. Quality check (raw reads) — `FastQC`  
2. Quality control and trimming — `fastp`  
3. Host (human) read removal — `Bowtie2`  
4. Taxonomic classification — `Kraken2`  
5. Species-level abundance estimation — `Bracken`  

---

## 📂 Input Data
For this demonstration, I worked with the paired-end FASTQ files of `ERR14218664`:

sra_data/ERR14218664/
├── ERR14218664_pass_1.fastq.gz
├── ERR14218664_pass_2.fastq.gz

yaml
Copy code

---

## 📦 Environment Setup

I created a conda environment and installed the required tools:

```bash
conda create -n wgs_demo
conda activate wgs_demo
conda install -c bioconda -c conda-forge fastqc fastp bowtie2 kraken2 bracken
Check installations (optional):

bash
Copy code
fastqc --version
fastp --version
bowtie2 --version
kraken2 --version
bracken --version
⚙️ Pipeline Steps (ERR14218664)
1️⃣ Quality Check (Raw Reads) — FastQC
bash
Copy code
mkdir -p fastqc_output

fastqc sra_data/ERR14218664/ERR14218664_pass_1.fastq.gz \
       sra_data/ERR14218664/ERR14218664_pass_2.fastq.gz \
       -o fastqc_output/
Output:

HTML and ZIP QC reports (fastqc_output/ERR14218664_pass_*.html)

Gives an overview of raw read quality before trimming

2️⃣ Quality Control and Trimming — FASTP
bash
Copy code
fastp -i sra_data/ERR14218664/ERR14218664_pass_1.fastq.gz \
      -I sra_data/ERR14218664/ERR14218664_pass_2.fastq.gz \
      -o fastp_output/ERR14218664_trimmed_R1.fastq.gz \
      -O fastp_output/ERR14218664_trimmed_R2.fastq.gz \
      --detect_adapter_for_pe -q 20 -u 20 -l 45 -w 8 \
      -j fastp_output/ERR14218664_fastp.json \
      -h fastp_output/ERR14218664_fastp.html
Output:

Trimmed FASTQ files

QC reports in JSON and HTML

3️⃣ Host Read Removal — Bowtie2
bash
Copy code
bowtie2 -x GRCh38_noalt_as/GRCh38_noalt_as \
        -1 fastp_output/ERR14218664_trimmed_R1.fastq.gz \
        -2 fastp_output/ERR14218664_trimmed_R2.fastq.gz \
        --un-conc-gz bowtie2_output/ERR14218664_nonhuman.fastq.gz \
        -S /dev/null -p 8
Output:

Non-human paired FASTQ files (ERR14218664_nonhuman.fastq.1.gz, ERR14218664_nonhuman.fastq.2.gz)

4️⃣ Taxonomic Classification — Kraken2
bash
Copy code
kraken2 --db kraken_database --threads 16 \
        --report kraken2_output/ERR14218664.k2report \
        --classified-out kraken2_output/ERR14218664_classified.fastq \
        --unclassified-out kraken2_output/ERR14218664_unclassified.fastq \
        --output kraken2_output/ERR14218664.kraken2.out \
        bowtie2_output/ERR14218664_nonhuman.fastq.1.gz \
        bowtie2_output/ERR14218664_nonhuman.fastq.2.gz
Output:

Classification report (ERR14218664.k2report)

Classified / unclassified reads

5️⃣ Abundance Estimation — Bracken
bash
Copy code
bracken -d kraken_database \
        -i kraken2_output/ERR14218664.k2report \
        -o bracken_output/ERR14218664.bracken.out \
        -r 100 -l S -t 10
Output:

Species-level abundance file (ERR14218664.bracken.out)


📌 Databases Used
Bowtie2: Human genome reference (GRCh38 no-alt analysis set)

Kraken2: UHGG v2.0.2 database (standard 8GB version)

Reference:
Almeida A., Mitchell A.L., Boland M. et al.
A unified catalog of 204,938 reference genomes from the human gut microbiome, Nature Biotechnology (2021)

✅ Key Takeaways
I first checked raw reads with FastQC before trimming with fastp.

Each tool produced outputs that I could interpret step by step.



Later, these steps can be automated for multiple samples in one pipeline.

