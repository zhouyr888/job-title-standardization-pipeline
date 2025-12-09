# Job Title Standardization Pipeline  
**TF-IDF · SBERT (pretrained) · SBERT (fine-tuned) · SOC-stratified evaluation · Canonical retrieval**

This repository implements an end-to-end pipeline for job-title standardization using both classical and neural embedding methods. The goal is to map noisy or emerging job titles to the correct canonical occupation using the O\*NET-SOC taxonomy. https://www.onetcenter.org/database.html#all-files:~:text=All%20Files-,O*NET%2030.0%20Database,in%20the%20O*NET%20core%20database%2C%20in%20one%20convenient%20ZIP%20archive.,-Excel

---

## Overview

Modern job titles evolve faster than official occupational taxonomies.  
This project evaluates whether **fine-tuning SBERT on occupational synonym pairs** helps or hurts two key objectives:

1. **Generalization to unseen job titles (coarse-level SOC accuracy)**  
2. **Precision in canonical title retrieval (fine-grained synonym matching)**  

The surprising finding: **fine-tuning improves generalization but weakens exact synonym retrieval** — revealing a fundamental multi-objective trade-off in embedding space.

## Pipeline Structure
- Full pipeline: Job_titles_project.ipynb
- Full datasets: Alternate Titles.xlsx

## 🔧 Methods

### **Text Normalization**
- Lowercasing  
- Removing punctuation + symbols  
- Normalizing common job-title patterns  
- Merging alternate titles with canonical O\*NET titles  

### **Models**
| Model | Purpose |
|-------|---------|
| **TF-IDF** | Lexical baseline, token-driven ranking |
| **SBERT-pre** | General semantic similarity |
| **SBERT-ft** | Fine-tuned on ONET synonym pairs (contrastive loss) |

### **Evaluation Protocols**
1. **Unseen-title generalization**  
   - SOC-stratified split (train/test)
   - Accuracy at SOC major-group level

2. **Canonical retrieval**  
   - Top-10 retrieval precision (P@10)
   - Embedding-space error analysis

3. **Qualitative failure cases (Table 3)**  
   - Lexical confusion  
   - Near-miss within same SOC family  
   - Novel-concept failures  

---

## Key Results (Summary)

- **Fine-tuned SBERT (SBERT-ft)** improves SOC generalization to **≈50% accuracy**, much higher than TF-IDF (≈22%).
- But **pretrained SBERT (SBERT-pre)** achieves **higher precision** for canonical retrieval (**7.53% vs 6.71% P@10**).
- Fine-tuning sharpens cluster boundaries → better broad grouping, weaker synonym discrimination.

---

## Interpretation

Fine-tuning for similarity on synonym pairs reshapes the embedding space:

- **Within-occupation distances shrink**
- **Between-occupation boundaries become sharper**

This helps place **new job titles** in the right SOC family but makes it harder to distinguish **multiple near-synonyms** within the same family.

