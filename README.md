# 🧬 Clinical Genomics: Variant Annotation Pipeline

## 📑 Project Overview

This repository contains the data, workflow, and results for a clinical genomics variant annotation pipeline. The objective of this project is to simulate a clinical diagnostic workflow by taking raw patient genetic variants (VCF format) and annotating them with clinical significance, phenotypic traits, and computational pathogenicity scores.

This project bridges the gap between raw genomic data (Whole Exome/Genome Sequencing) and actionable medical insights by leveraging industry-standard genomic databases.

---

## 🔬 Clinical Variants Analyzed

The following six pathogenic variants (rare and common genetic disorders) were selected for analysis. They represent different inheritance patterns and systemic impacts:

| Disease / Condition | Gene  | Variant (Consequence) | dbSNP ID | Inheritance |
|--------------------|-------|------------------------|----------|------------|
| Phenylketonuria (PKU) | PAH   | c.1222C>T (p.Arg408Trp) | rs5030858 | Autosomal Recessive |
| Cystic Fibrosis | CFTR  | c.1652G>A (p.Gly551Asp) | rs75527207 | Autosomal Recessive |
| Achondroplasia | FGFR3 | c.1138G>A (p.Gly380Arg) | rs28931614 | Autosomal Dominant |
| Sickle Cell Anemia | HBB   | c.20A>T (p.Glu7Val) | rs334 | Autosomal Recessive |
| Hereditary Hemochromatosis | HFE   | c.845G>A (p.Cys282Tyr) | rs1800562 | Autosomal Recessive |
| Hereditary Breast Cancer | BRCA1 | c.5095C>T (p.Arg1699Trp) | rs28897696 | Autosomal Dominant |

---

## 📂 Repository Contents

- **assignment_data.xlsx**  
  Master data dictionary containing:
  - Variant details  
  - OMIM phenotypic extraction  
  - ACMG/AMP classifications  
  - Screenshots of computational pathogenicity scores  

- **patient_variants_raw.vcf**  
  Raw VCF file containing genomic coordinates for the six variants (GRCh38 assembly).  
  Simulates raw sequencing output.

- **patient_variants.vcf**  
  Final annotated VCF file cross-referenced with ClinVar to append clinical significance.

---

## 🛠️ Step-by-Step Reproducibility Guide

This section ensures full reproducibility for reviewers and non-bioinformaticians.

---

### Step 1: Clinical Data & Phenotype Extraction

**ClinVar**
1. Navigate to the NCBI ClinVar database.
2. Search for each gene/variant.
3. Extract:
   - Molecular consequence  
   - Associated conditions  
   - Clinical significance (Pathogenic/Benign)

**OMIM**
1. Navigate to the OMIM database.
2. Search the gene name.
3. Go to the **"Allelic Variants"** section.
4. Extract phenotypic traits associated with the exact mutation.

---

### Step 2: Computational Pathogenicity Scoring

To predict functional impact, AI-driven scores were extracted using the UCSC Genome Browser.

1. Navigate to UCSC Genome Browser (Assembly: Human GRCh38/hg38).
2. Search variants using dbSNP identifiers:
   - rs5030858  
   - rs75527207  
   - rs28931614  
   - rs334  
   - rs1800562  
   - rs28897696  

3. Enable the following tracks (set to **Full**):
   - AlphaMissense (under Phenotype and Literature)
   - REVEL (under Variation)

4. Hover over the color-coded variant blocks to extract exact computational scores.

> Screenshots of extracted scores are archived in `assignment_data.xlsx`.

---

### Step 3: VCF Generation

A standard **VCFv4.2** file was manually constructed using GRCh38 coordinates:

```vcf
##fileformat=VCFv4.2
##source=Assignment_2_Clinical_Genomics
##reference=GRCh38
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO
12	102840493	rs5030858	C	T	.	PASS	.
7	117587806	rs75527207	G	A	.	PASS	.
4	1804392	rs28931614	G	A	.	PASS	.
11	5227002	rs334	T	A	.	PASS	.
6	26093141	rs1800562	G	A	.	PASS	.
17	43070967	rs28897696	C	T	.	PASS	.
