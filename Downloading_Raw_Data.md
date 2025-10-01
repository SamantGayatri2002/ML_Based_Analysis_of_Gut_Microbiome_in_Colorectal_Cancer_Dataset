# 🎯 SRA Data Processing

As part of the the workshop, we were introduced to the basics of working with raw sequencing data from the **Sequence Read Archive (SRA)**.  
Due to limited computational resources, we could not process the entire dataset. Instead, we worked with **three Run IDs** as a demonstration:

- `ERR14218891`  
- `ERR14218664`  
- `ERR14219004`  

The same steps can be applied to all runs, and it is also possible to automate the process with a simple script.  

---

## 🔧 Setting Up the Environment

We first created a Conda environment and installed **SRA Tools**, which are necessary for downloading and processing `.sra` files.

```bash
conda create -n sra python=3.8 -y
conda activate sra
conda install -c bioconda sra-tools -y
```
## 📂 Downloading SRA Files
A folder was created to store the downloaded data:

```bash
mkdir -p sra_data
```
Then we used the prefetch command to download .sra files for the three example runs:

```bash
prefetch --output-directory sra_data ERR14218891
prefetch --output-directory sra_data ERR14218664
prefetch --output-directory sra_data ERR14219004
```
🔄 Converting to FASTQ
Next, we converted the .sra files into compressed FASTQ format using fastq-dump.

For each run, the command looked like this:
```
fastq-dump --outdir sra_data/ERR14218891 --gzip --skip-technical \
  --readids --read-filter pass --dumpbase --split-files --clip \
  sra_data/ERR14218891/ERR14218891.sra
```
We repeated the same for the other two runs (ERR14218664, ERR14219004).

The output consisted of paired FASTQ files (*_1.fastq.gz and *_2.fastq.gz).

👀 Checking the Files
To verify, we listed the contents of each folder:

```bash
ls -lh sra_data/ERR14218891/
ls -lh sra_data/ERR14218664/
ls -lh sra_data/ERR14219004/
```

📌 Reflections
* Since we were working on limited systems, we could not process the entire dataset.
* Using three runs gave us a good understanding of how the data download and conversion steps work.

I learned that the same commands can be applied to all Run IDs, and for large datasets it’s more efficient to use a loop or automation script.



Here’s a ready-to-use bash script (sra_download.sh) that automates everything we did in the workshop for multiple Run IDs. You can upload this directly to your GitHub repo.
```
#!/bin/bash
# ============================================================
# SRA Download and FASTQ Conversion Script
# Author: Gayatri Sunil Samant
# Purpose: Automate downloading .sra files from NCBI SRA and
#          converting them into compressed FASTQ files
# ============================================================

# ----------- CONFIGURATION -----------
# List of Run IDs (you can add more as needed)
RUN_IDS=("ERR14218891" "ERR14218664" "ERR14219004")

# Output directory
OUTPUT_DIR="sra_data"

# -------------------------------------

echo "🚀 Starting SRA download and conversion pipeline..."

# Step 1: Create output directory if it doesn’t exist
mkdir -p $OUTPUT_DIR

# Step 2: Loop through each Run ID
for RUN in "${RUN_IDS[@]}"; do
    echo "--------------------------------------------"
    echo "📥 Processing Run ID: $RUN"
    
    # Step 2a: Download .sra file
    prefetch --output-directory $OUTPUT_DIR $RUN
    
    # Step 2b: Create subfolder for the run
    mkdir -p $OUTPUT_DIR/$RUN
    
    # Step 2c: Convert to FASTQ (paired-end, gzipped)
    fastq-dump --outdir $OUTPUT_DIR/$RUN --gzip --skip-technical \
      --readids --read-filter pass --dumpbase --split-files --clip \
      $OUTPUT_DIR/$RUN/$RUN.sra
    
    echo "✅ Finished processing $RUN"
done

# Step 3: Show summary
echo "--------------------------------------------"
echo "🎉 All downloads and conversions completed!"
echo "Check the $OUTPUT_DIR folder for FASTQ files."
ls -lh $OUTPUT_DIR/*/*.fastq.gz
```
## 🔧 How to Use

Save the script as sra_download.sh in your GitHub repo.

Give it execution permission:
```
chmod +x sra_download.sh
```

Run it:
```
./sra_download.sh
```

It will automatically download the .sra files, convert them to .fastq.gz, and organize them neatly in subfolders.


This  helped me understand:
- How to set up a dedicated environment for bioinformatics tools.
- The process of downloading raw .sra files.
- Converting .sra files into usable FASTQ format.
- The importance of automation in handling large datasets.

Although this was just a small-scale demonstration, I now have the confidence to scale up the process for larger projects in shotgun metagenomics.
