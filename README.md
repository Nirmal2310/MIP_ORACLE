# MIP_ORACLE
A software to filter and identify unique target regions with diagnostic significance in antimicrobial resistance genes, and various other pathogen genomes.
# Table of contents
  1. Demo-Preview
  2. Requirements
  3. Design
---
# Demo-Preview
Molecular Inversion Probes(MIPs), are single-stranded DNA molecules containing two complementary regions which flank the target DNA. 
These molecules often have a Fluorophore, DNA barcode, or Molecular tag for unique identification.

![MIP_example](https://github.com/SakshiPandey97/MIP_ORACLE/assets/59496870/9d92d545-ffe3-42c6-9125-0c3271ccd35f)

Rough Design Outline- 
1. Start with all possible MIPs by moving along the strand one base pair at a time. 
2. Design MIPs for both the forward and reverse strands so that we have the highest probability of binding and then proceed to filter them according to three user specified criteria:
  Temperature
  GC Content
  Nucleotide Repeats
3. Following this, further filter the MIPs by BLASTing them against the host genome(human).
4. In order to further increase the probability of the MIP binding to the correct target region BLAST them against the non-redundant nucleotides database as well. Filter out any MIPs which match other organisms.

Demo-
1. Obtain sequences of interest in a FASTA format, make sure the organism name is present in the definition line of each sequence. 
2. Following this download all the program files and store them in the same directory as the FASTA file.
3. Fill out the requirements to filter MIPs in the config file provided. The MIPs within the ranges given will be accepted. ex. all MIPs with 45<temp<70 will be taken.

![image](https://user-images.githubusercontent.com/59496870/133621729-c870017d-8ed5-4c49-afe8-32ca1b00bf01.png)

4.  Run the shell script provided as so:
```bash
bash MIP_ORACLE.sh -i Trial_File -o trial_final_results -l mip_oracle -j /DATA/databases/blast/nt
```
6.  nohup can also be used:
```bash
nohup bash MIP_ORACLE.sh -i Lactobacillus_fermentum_16S -o lacto16S_final_results -l mip_oracle -j /DATA/databases/blast/nt > lacto16S_log.out &
```
9.  The following files will be generated(The first eight files will be in a folder called LOG_FILES):
      1. The first file will contain all possible MIPs for the sequences provided.
      2. The second and third file will contain Passable MIPs(The MIPs which met user requirements as per the config file), and Eliminated MIPs(MIPs which were filtered out).
      3. The fourth file is the BLAST input containing arm1+target+arm2 sequences.
      4. The fifth and sixth files are the .xml result files from BLAST.
      5. The seventh file will contain the parsed BLAST results about each MIP, and the eighth file will have the filtered results.
      6. Lastly the final result file will be generated in an excel format.
![image](https://user-images.githubusercontent.com/59496870/132258338-4d4c583a-835c-4470-99da-da8675d42928.png)    


# Requirements
Nucleotide BLAST 2.12.0 + with the nt database.
  
Python 3.6 and the following python packages:
1. pandas=1.1.5
2. biopython=1.70
3. configparser
4. regex
5. xlsxwriter
6. openpyxl

Users can install the required packages through conda using the following command

```bash
conda create -n mip_oracle --file mip_oracle_env.txt
```

# Design
![flowchart](https://github.com/SakshiPandey97/MIP_ORACLE/assets/59496870/a9fbe16b-1f08-4d64-afd3-7ea3934e036e)
