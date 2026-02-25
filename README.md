# 🧬 Clinical Genomics: End-to-End Variant Annotation Pipeline

## 📌 Project Overview

This repository demonstrates a fully reproducible **clinical variant annotation workflow** that simulates how raw genomic variants from a patient sequencing run are transformed into clinically interpretable results.

Starting from a raw **VCF (Variant Call Format)** file, this pipeline:

1. Extracts clinical interpretations from authoritative databases  
2. Retrieves phenotype associations  
3. Integrates computational pathogenicity scores  
4. Produces an annotated VCF enriched with disease and clinical significance data  

The goal is to bridge the gap between **raw sequencing output (WES/WGS)** and **actionable clinical insights** using standardized genomic resources and best practices used in translational genomics.

---

# 🧪 Clinical Context

In real-world diagnostics, sequencing laboratories generate thousands of variants per patient. These variants must be:

- Cross-referenced with clinical databases
- Evaluated for inheritance patterns
- Assessed for pathogenic potential
- Linked to known phenotypes
- Classified under ACMG/AMP guidelines

This project simulates that workflow using six well-characterized pathogenic variants representing both **autosomal dominant** and **autosomal recessive** disorders.

---

# 🔬 Variants Analyzed

The following clinically significant variants were selected to represent diverse disease mechanisms and inheritance patterns:

| Disease / Condition | Gene  | Variant (HGVS) | dbSNP ID | Inheritance Pattern |
|--------------------|-------|----------------|----------|---------------------|
| Phenylketonuria (PKU) | PAH   | c.1222C>T (p.Arg408Trp) | rs5030858 | Autosomal Recessive |
| Cystic Fibrosis | CFTR  | c.1652G>A (p.Gly551Asp) | rs75527207 | Autosomal Recessive |
| Achondroplasia | FGFR3 | c.1138G>A (p.Gly380Arg) | rs28931614 | Autosomal Dominant |
| Sickle Cell Anemia | HBB   | c.20A>T (p.Glu7Val) | rs334 | Autosomal Recessive |
| Hereditary Hemochromatosis | HFE   | c.845G>A (p.Cys282Tyr) | rs1800562 | Autosomal Recessive |
| Hereditary Breast Cancer | BRCA1 | c.5095C>T (p.Arg1699Trp) | rs28897696 | Autosomal Dominant |

These variants were chosen because:

- They are well-documented in clinical literature  
- They have known pathogenic classifications  
- They span metabolic, hematologic, skeletal, and oncologic disorders  
- They represent different molecular mechanisms (missense substitutions)

---

# 📂 Repository Structure

### 📄 `assignment_data.xlsx`

Master annotation workbook containing:

- Variant metadata  
- Molecular consequences  
- OMIM phenotype extraction  
- ACMG/AMP classifications  
- AlphaMissense scores  
- REVEL scores  
- Screenshots documenting computational evidence  

This file acts as the structured evidence log for all annotations.

---

### 📄 `patient_variants_raw.vcf`

Simulated raw output from a patient sequencing run (GRCh38 assembly).

This file contains only:

- Chromosome
- Genomic position
- Reference allele
- Alternate allele

No clinical interpretation is included at this stage.

---

### 📄 `patient_variants.vcf`

Final annotated VCF produced after:

- Database cross-referencing
- ClinVar integration
- Phenotype mapping

This file represents what a clinical genomics team would review before issuing a report.

---

# 🔁 Full Reproducibility Guide

This section ensures any reviewer can replicate the workflow without prior bioinformatics experience.

---

## Step 1: Clinical Database Annotation

### 🔎 ClinVar Extraction

Purpose: Determine clinical significance.

Procedure:

1. Navigate to NCBI ClinVar
2. Search each variant using:
   - Gene name
   - HGVS notation
   - dbSNP ID
3. Extract:
   - Clinical significance (Pathogenic / Likely Pathogenic / Benign)
   - Associated conditions
   - Review status

ClinVar provides consensus interpretations from clinical laboratories.

---

### 🧬 OMIM Phenotype Extraction

Purpose: Identify phenotypic manifestations.

Procedure:

1. Navigate to OMIM (Online Mendelian Inheritance in Man)
2. Search by gene name
3. Open the **"Allelic Variants"** section
4. Locate the exact mutation
5. Extract:
   - Phenotypic presentation
   - Disease severity
   - Clinical variability

OMIM provides deep genotype–phenotype correlations.

---

## Step 2: Computational Pathogenicity Scoring

Clinical databases provide curated interpretations, but computational tools estimate how damaging a variant may be to protein structure or function.

### Tool Used: UCSC Genome Browser (GRCh38/hg38)

Search using dbSNP IDs:

- rs5030858  
- rs75527207  
- rs28931614  
- rs334  
- rs1800562  
- rs28897696  

Enable tracks:

- **AlphaMissense** (deep learning pathogenicity model)
- **REVEL** (ensemble missense pathogenicity predictor)

Hover over the variant to extract:

- AlphaMissense score
- REVEL score

These scores provide quantitative evidence supporting ACMG criteria (e.g., PP3 computational evidence).

All extracted scores are documented in `assignment_data.xlsx`.

---

## Step 3: VCF Generation

A standardized VCFv4.2 file was constructed using GRCh38 coordinates.

```vcf
##fileformat=VCFv4.2
##source=Assignment_2_Clinical_Genomics
##reference=GRCh38
#CHROM  POS        ID          REF ALT QUAL FILTER INFO
12      102840493  rs5030858   C   T   .    PASS   .
7       117587806  rs75527207  G   A   .    PASS   .
4       1804392    rs28931614  G   A   .    PASS   .
11      5227002    rs334       T   A   .    PASS   .
6       26093141   rs1800562   G   A   .    PASS   .
17      43070967   rs28897696  C   T   .    PASS   .
```
---

---

## Step 4: Variant Annotation via Ensembl VEP

Since the NCBI Variation Reporter has been deprecated, this workflow uses **Ensembl Variant Effect Predictor (VEP)** for modern, reproducible annotation.

### Objective
To enrich the raw VCF file with:
- Clinical significance
- Disease/phenotype associations
- Transcript-level consequences
- Database cross-references

### Procedure

1. Navigate to the **Ensembl VEP Web Tool (Human GRCh38)**.
2. Upload the file:  
   `patient_variants_raw.vcf`
3. Under **Additional Annotations**, enable:
   - **Phenotypes and/or disease/trait data**
4. Execute the analysis.
5. Export the annotated output as:  
   `patient_variants.vcf`

### Output

The resulting `patient_variants.vcf` file includes:
- ClinVar annotations
- Clinical significance classifications
- Associated disease terms
- Functional consequence predictions

This file represents the fully annotated clinical dataset ready for interpretation.

---

## 🧰 Databases & Resources Used

### ClinVar (NCBI)
Provides clinically interpreted genomic variants and their relationships to human health.

### OMIM
Comprehensive resource linking genes to inherited phenotypes and disease mechanisms.

### UCSC Genome Browser
Used to retrieve computational pathogenicity scores:
- AlphaMissense
- REVEL

### Ensembl Variant Effect Predictor (VEP)
Automated tool for variant annotation, consequence prediction, and database integration.

---

## 📊 Final Deliverables

- `assignment_data.xlsx` — Evidence and scoring documentation
- `patient_variants_raw.vcf` — Unannotated input variant file
- `patient_variants.vcf` — Fully annotated clinical output file

---

## ⚠️ Disclaimer

This project is intended for academic and educational purposes only.  
It is not designed for clinical diagnosis or medical decision-making.
