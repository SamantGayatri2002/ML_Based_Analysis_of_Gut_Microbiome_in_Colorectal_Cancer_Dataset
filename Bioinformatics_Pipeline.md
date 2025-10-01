# After Downloading the Raw data, we can go through end-to-end shotgun metagenomic pipeline .


Here, I a ran each step **individually** to understand the tools and their outputs clearly. If you have enough computational power to handle large amount of datasets and its dependencies, these steps can be combined into an automated script to reduce the manual work.

---

## 🔧 Pipeline Overview
This  pipeline covers the following steps:

1. Quality check (raw reads) — `FastQC`  
2. Quality control and trimming — `fastp`  
3. Host (human) read removal — `Bowtie2`  
4. Taxonomic classification — `Kraken2`  
5. Species-level abundance estimation — `Bracken`  


## 📂 Input Data
For this demonstration, I worked with the paired-end FASTQ files of `ERR14218664`:
```
sra_data/ERR14218664/
├── ERR14218664_pass_1.fastq.gz
├── ERR14218664_pass_2.fastq.gz
```

-----

## 📦 Environment Setup

I created a conda environment and installed the required tools:

```bash
conda create -n wgs_demo
conda activate wgs_demo
conda install -c bioconda -c conda-forge fastqc fastp bowtie2 kraken2 bracken
```

Check installations (optional):

```
fastqc --version
fastp --version
bowtie2 --version
kraken2 --version
bracken --version
```
-------
## 📌 Databases Used
- Bowtie2: Human genome reference (GRCh38 no-alt analysis set)
- Kraken2: UHGG v2.0.2 database (standard 8GB version)
- To install these databases please go to the bottom section of this file
-------

# ⚙️ Pipeline Steps (ERR14218664)

## 1️⃣ Quality Check (Raw Reads) — FastQC
```
mkdir -p fastqc_output 

fastqc sra_data/ERR14218664/ERR14218664_pass_1.fastq.gz \
       sra_data/ERR14218664/ERR14218664_pass_2.fastq.gz \
       -o fastqc_output/
```
Output:



HTML and ZIP QC reports (fastqc_output/ERR14218664_pass_*.html)

Gives an overview of raw read quality before trimming

-----

## 2️⃣ Quality Control and Trimming — FASTP
```
mkdir -p fastp_output 

fastp -i sra_data/ERR14218664/ERR14218664_pass_1.fastq.gz \
      -I sra_data/ERR14218664/ERR14218664_pass_2.fastq.gz \
      -o fastp_output/ERR14218664_trimmed_R1.fastq.gz \
      -O fastp_output/ERR14218664_trimmed_R2.fastq.gz \
      --detect_adapter_for_pe -q 20 -u 20 -l 45 -w 8 \
      -j fastp_output/ERR14218664_fastp.json \
      -h fastp_output/ERR14218664_fastp.html
```
Output:


Trimmed FASTQ files

QC reports in JSON and HTML

------

## 3️⃣ Host Read Removal — Bowtie2

- First download the Human Reference Genome in the **Home directory** from the link given in the bottom .
- It can be simple done using the copy adress of the link given in the bottom of the file
- paste the copied adress infront of wget command
```
wget < copied link >
```
- It will Automatically create a folder called GRCh38_noalt_as.zip
- Unzip this folder using gunzip command
```
gunzip GRCh38_noalt_as.zip
```
- Inside the folder you should see the following files
<img width="1183" height="357" alt="image" src="https://github.com/user-attachments/assets/7cf2f967-b056-4242-8981-880a425573fb" />
- Then continue to run the below command
   
```
mkdir -p bowtie2_output

bowtie2 -x GRCh38_noalt_as/GRCh38_noalt_as \
        -1 fastp_output/ERR14218664_trimmed_R1.fastq.gz \
        -2 fastp_output/ERR14218664_trimmed_R2.fastq.gz \
        --un-conc-gz bowtie2_output/ERR14218664_nonhuman.fastq.gz \
        -S /dev/null -p 8
```
Output:

Non-human paired FASTQ files (ERR14218664_nonhuman.fastq.1.gz, ERR14218664_nonhuman.fastq.2.gz)

-----

## 4️⃣ Taxonomic Classification — Kraken2

In **home directory** create a folder called  kraken_database
```
mkdir -p  kraken_database
```
- Inside this folder , you have to download all the databases of kraken which can be accessed through the link given in the bottom.
- Use wget command to download each kracken databases
```
wget < copied link >
```
- This will create a **k2_standard_08_GB_20250714.tar.gz** file
```
cd  kraken_database
wget < copy paste the adress of the database>
```
- Do the same for all the kraken databases
- The kraken_database folder should contain the following files

Now come back one step back by
```
cd ..
```
and run the below command
```
mkdir -p kraken2_output

kraken2 --db kraken_database --threads 16 \
        --report kraken2_output/ERR14218664.k2report \
        --classified-out kraken2_output/ERR14218664_classified.fastq \
        --unclassified-out kraken2_output/ERR14218664_unclassified.fastq \
        --output kraken2_output/ERR14218664.kraken2.out \
        bowtie2_output/ERR14218664_nonhuman.fastq.1.gz \
        bowtie2_output/ERR14218664_nonhuman.fastq.2.gz
```
Output:

Classification report (ERR14218664.k2report)

Classified / unclassified reads

-----

## 5️⃣ Abundance Estimation — Bracken
```
mkdir -p bracken_output

bracken -d kraken_database \
        -i kraken2_output/ERR14218664.k2report \
        -o bracken_output/ERR14218664.bracken.out \
        -r 100 -l S -t 10
```
Output:

Species-level abundance file (ERR14218664.bracken.out)

----

- Each tool produced outputs that I could interpret step by step.
<img width="773" height="232" alt="image" src="https://github.com/user-attachments/assets/e5259859-1f37-4454-bc43-abbf706a3e6d" />



- After the entire pipeline the linux terminal should contain all the above folders
- We can also automate these steps  for multiple samples in one pipeline.


---

## 📦 Database Requirements

This workflow requires pre-built reference databases for **host removal** and **taxonomic classification**.  
Below are the resources I used during my learning:

### 🔹 1. Kraken2 Database
- **Based on:** Unified Human Gastrointestinal Genome (UHGG) v2.0.2  
- **Use case:** Taxonomic classification of non-human reads  
- **RAM requirement:** ~8 GB  
- **Download link:**  
  [k2_standard_08_GB_20250714.tar.gz](https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08_GB_20250714.tar.gz)  

📄 **Reference:**  
Almeida A., Mitchell A.L., Boland M. *et al.*  
*A unified catalog of 204,938 reference genomes from the human gut microbiome.*  
**Nature Biotechnology (2021).**

---

### 🔹 2. Bowtie2 Human Reference Genome
- **Reference genome:** Human GRCh38 no-alt analysis set (indexed for Bowtie2)  
- **Use case:** Removal of human (host) reads prior to metagenomic classification  
- **Download link:**  
➡️ [GRCh38 no-alt Bowtie2 Index](https://genome-idx.s3.amazonaws.com/bt/GRCh38_noalt_as.zip)

---

⚠️ **Note for learners (like me):**  
- These databases are **large** and downloading can take significant time and storage.  
- For hands-on practice with a single sample, the **8 GB Kraken2 database** is a good balance between accuracy and resource requirements.  
- The Bowtie2 human index is required only when working with **host-associated samples** (e.g., human gut microbiome).  

---


