# Nextflow
Basic steps to learn nextflow according to the course Bioinformatics for Biologists: Analyzing and Interpreting Genomics Datasets Wellcome Connecting Science using the https://nf-co.re/ pipeline

## Viralrecon Workflow 
This repository contains scripts and instructions for analyzing SARS-CoV-2 genomic data using the nf-core/viralrecon Nextflow pipeline. The workflow is designed for amplicon-based sequencing data and provides comprehensive analysis including variant calling, consensus sequence generation, and annotation.

Setting up the Environment
1. Through Conda:
If you haven't already, set up your conda channels:
```markdown
bash
Copy code
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```
Now create an environment called nextflow and install Nextflow in it:
```markdown
bash
Copy code
conda create --name nextflow nextflow
```
Before using Nextflow, activate the nextflow environment:
```markdown
bash
Copy code
conda activate nextflow
```
To check if it's working, run the following command:
```markdown
bash
Copy code
nextflow help
```
## Preparing the Data
We'll use the dataset of benchmarked SARS-CoV-2 WGS for analysis. Follow the provided instructions for downloading and preparing the data.

Activate the MOOC environment:
```markdown
bash
Copy code
conda activate MOOC
```
Create a text file named samples.txt in your working directory and populate it with ENA accessions.

Use a for loop to run fastq-dump on each accession:
```markdown
bash
Copy code
for i in $(cat samples.txt); do
    fastq-dump --split-files $i;
done
```
Compress the downloaded fastq files:
```markdown
bash
Copy code
gzip *.fastq
```
Create a directory called data and move the fastq files there:
```markdown
bash
Copy code
mkdir data
mv *.fastq.gz data
```
## Creating the samplesheet.csv File
The nf-core pipeline requires a CSV file (samplesheet.csv) containing sample names and the location of the fastq files. Use the provided Python script to generate the samplesheet automatically:
```markdown
bash
Copy code
wget -L https://raw.githubusercontent.com/nf-core/viralrecon/master/bin/fastq_dir_to_samplesheet.py
python3 fastq_dir_to_samplesheet.py data samplesheet.csv -r1 _1.fastq.gz -r2 _2.fastq.gz
```
## Running Viralrecon
Before running the pipeline, activate the nextflow environment:
```markdown
bash
Copy code
conda activate nextflow
```
Now, execute the following command to run viralrecon:
```markdown
bash
Copy code
nextflow run nf-core/viralrecon -profile conda \
--max_memory '12.GB' --max_cpus 4 \
--input samplesheet.csv \
--outdir results/viralrecon \
--protocol amplicon \
--genome 'MN908947.3' \
--primer_set artic \
--primer_set_version 3 \
--skip_kraken2 \
--skip_assembly \
--skip_pangolin \
--skip_nextclade \
--platform Illumina
```
For more detailed information about the parameters, refer to the viralrecon documentation.

## Notes
Adjust paths in the scripts based on your file locations.
Customize the workflow for different samples or datasets as needed.
Ensure you have the necessary permissions to execute the scripts.
Feel free to explore additional parameters for viralrecon based on your analysis requirements.
If you have any questions or encounter issues, don't hesitate to reach out. Happy analyzing!
