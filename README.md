# B-Chromosome Detection Pipeline Using Reference-Derived k-mers

This repository contains a computational pipeline for identifying and estimating the copy number of **B chromosomes (chrB)** in *Distichlis* whole-genome sequencing (WGS) datasets using chromosome-specific **k-mers**.

The workflow builds a high-confidence database of B chromosome-specific k-mers from reference genomes and screens thousands of WGS samples to predict B chromosome copy number. Predictions can be validated experimentally using fluorescence in situ hybridization (FISH).

---

## Workflow Overview

![Pipeline Workflow](sandbox:/mnt/data/8714E222-33A5-475F-93E6-0277D60A42FB.png)

---

## Pipeline

### Step 1. Assemble B chromosomes (chrB)

High-quality *Distichlis* genome assemblies are used to identify and curate B chromosome sequences.

Quality control includes:

- Removing Ns and assembly gaps
- Retaining the longest chrB contigs
- Standardizing chromosome orientation
- Annotating repeat regions and telomeres

---

### Step 2. Build candidate chrB k-mers

The curated B chromosome sequences are converted into **51-mers** using **Jellyfish**.

Candidate chrB k-mers are obtained by intersecting the k-mer sets from multiple independently assembled B chromosomes.

Output:

- Common chrB k-mers shared among reference genomes

---

### Step 3. Remove A chromosome k-mers

To ensure high specificity, candidate chrB k-mers are compared against the A chromosomes from all reference genomes.

Any k-mer found in an A chromosome is removed.

Output:

- High-confidence B chromosome-specific k-mer database (`DBk`)

---

### Step 4. Screen WGS samples

Each WGS sample is queried against the chrB-specific k-mer database.

For sample *i*,

\[
K_i=\frac{C_i}{N_i-k+1}\times10^9
\]

where

- \(C_i\) = number of chrB-specific k-mer hits
- \(N_i\) = total sequenced bases
- \(k=51\)

The normalized value is converted into an estimated B chromosome copy number using reference distributions from B-negative samples.

Example classification:

| Predicted value | Classification |
|-----------------|----------------|
| <0.5 | 0B |
| 0.5–1.5 | 1B |
| 1.5–2.5 | 2B |
| ... | ... |

---

### Step 5. Experimental validation

Predicted copy numbers are validated using fluorescence in situ hybridization (FISH).

Two probe sets are designed:

- chrB-specific probes
- Centromere-specific probes

FISH is performed on root tip or meiotic cells and compared with computational predictions.

---

## Input

- High-quality *Distichlis* reference genomes
- Curated B chromosome assemblies
- Whole-genome sequencing reads (FASTQ)

---

## Output

- chrB-specific k-mer database
- Normalized chrB k-mer counts
- Predicted B chromosome copy number
- FISH validation results

---

## Software

- Jellyfish
- Python
- Bash
- BEDTools (optional)
- SAMtools (optional)

---

## Applications

- Large-scale B chromosome screening
- Population genetics
- Cytogenomics
- Evolutionary genomics
- High-throughput B chromosome detection

---

## Citation

If you use this pipeline, please cite the associated publication once available.
