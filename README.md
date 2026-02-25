# 🧬 Clinical Genomics: Variant Annotation Pipeline

## 📑 Project Overview

This repository contains the data, workflow, and results for a clinical genomics variant annotation pipeline. The objective of this project is to simulate a clinical diagnostic workflow by taking raw patient genetic variants (in VCF format) and annotating them with clinical significance, phenotypic traits, and computational pathogenicity scores.

This project bridges the gap between raw genomic data (from Whole Exome/Genome Sequencing) and actionable medical insights by utilizing industry-standard genomic databases.

## 🔬 Clinical Variants Analyzed

The following three pathogenic variants were selected for analysis. They represent different genetic disorders, inheritance patterns, and systemic impacts:

| Disease / Condition | Gene | Variant (Consequence) | dbSNP ID | Inheritance | 
| ----- | ----- | ----- | ----- | ----- | 
| **Phenylketonuria (PKU)** | `PAH` | c.1222C>T (p.Arg408Trp) | `rs5030858` | Autosomal Recessive | 
| **Cystic Fibrosis** | `CFTR` | c.1652G>A (p.Gly551Asp) | `rs75527207` | Autosomal Recessive | 
| **Achondroplasia** | `FGFR3` | c.1138G>A (p.Gly380Arg) | `rs28931614` | Autosomal Dominant | 

## 📂 Repository Contents

* 📄 **`assignment_data.xlsx`**: The master data dictionary. Contains all variant details, OMIM phenotypic extraction, ACMG/AMP classifications, and visual proof (screenshots) of computational pathogenicity scores.

* 📄 **`patient_variants_raw.vcf`**: The raw Variant Call Format (VCF) file containing the genomic coordinates for the three variants (GRCh38 assembly). This simulates the raw output from a patient's DNA sequencing run.

* 📄 **`patient_variants.vcf`**: The final, annotated VCF file. This file has been cross-referenced with the ClinVar database to append clinical significance directly to the patient's genetic data.

## 🛠️ Step-by-Step Reproducibility Guide

*For reviewers and non-bioinformaticians, follow these steps to perfectly reproduce this workflow.*

### Step 1: Clinical Data & Phenotype Extraction

1. **ClinVar:** Navigate to the [NCBI ClinVar Database](https://www.ncbi.nlm.nih.gov/clinvar/). Search for each gene/variant to identify the exact molecular consequence, associated conditions, and clinical significance (Pathogenic/Benign).

2. **OMIM:** Navigate to the [Online Mendelian Inheritance in Man (OMIM)](https://www.omim.org/) database. Search the gene name, navigate to the "Allelic Variants" section, and extract the specific phenotypic traits observed in patients with this exact mutation.

### Step 2: Computational Pathogenicity Scoring

To predict how damaging a mutation is to a protein's structure and function, we extract AI-driven scores using the **UCSC Genome Browser**.

1. Navigate to the [UCSC Genome Browser (Assembly: Human GRCh38/hg38)](https://genome.ucsc.edu/cgi-bin/hgGateway).

2. Search for the variants using their specific dbSNP identifiers (`rs5030858`, `rs75527207`, `rs28931614`) to ensure exact coordinate targeting.

3. Scroll down to the track controls and enable the following tracks to `full`:

   * **AlphaMissense** (under *Phenotype and Literature*)

   * **REVEL** (under *Variation*)

4. **Data Extraction:** Hover over the color-coded blocks aligned with the mutated allele to reveal the exact computational scores. *(Note: Screenshots of these scores are archived in the provided Excel file).*

### Step 3: VCF Generation

A standard `VCFv4.2` file was manually constructed using the GRCh38 genomic coordinates identified in the previous steps.

```text
##fileformat=VCFv4.2
##source=Assignment_2_Clinical_Genomics
##reference=GRCh38
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO
12	102840493	rs5030858	C	T	.	PASS	.
7	117587806	rs75527207	G	A	.	PASS	.
4	1804392	rs28931614	G	A	.	PASS	.
Step 4: Variant Annotation (ClinVar Integration)
⚠️ Methodology Note: The NCBI Variation Reporter web interface has been deprecated by the NIH. To ensure robust, modern, and reproducible annotation without requiring local Linux command-line environments, this pipeline utilizes the industry-standard Ensembl Variant Effect Predictor (VEP).

Navigate to the Ensembl VEP Web Tool (Human GRCh38).

Upload the patient_variants_raw.vcf file.

Under Additional Annotations, enable the Phenotypes and/or disease/trait data setting. This specifically commands the tool to cross-reference the uploaded variants against the ClinVar database.

Execute the tool and export the resulting annotated VCF file.

🧰 Databases & Resources Used
ClinVar (NCBI): Aggregates information about genomic variation and its relationship to human health.

OMIM: Comprehensive compendium of human genes and genetic phenotypes.

UCSC Genome Browser: Interactive viewer for genomic data, used here for AlphaMissense and REVEL deep-learning pathogenicity models.

Ensembl VEP: Analyzes, annotates, and prioritizes genomic variants.

Disclaimer: This repository is for academic simulation and educational purposes only. It is not intended for actual medical diagnosis or clinical use
