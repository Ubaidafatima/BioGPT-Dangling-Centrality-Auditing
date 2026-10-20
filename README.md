# BioGPT Dangling Centrality Auditing

Reproducibility code and derived results for the manuscript:

**Graph-Theoretic Analysis of BioGPT Attention in Biomedical Question Answering Using Dangling Centrality and Intervention-Based Faithfulness**

**Authors:** Ubaida Fatima and Mustafain Ali

## Overview

This repository contains the computational notebooks and aggregate derived results used to study BioGPT self-attention as a token-level graph. The framework applies Dangling Centrality (`phi_C`) to quantify structural dependence in the attention network and evaluates an uncertainty-aware Attention-Topology Influence Score (`Omega_MH`). An intervention experiment tests output sensitivity by deleting highly ranked question-stem words and comparing them with raw-attention and matched random-deletion baselines.

The framework is intended for research auditing and interpretability. It is **not** a clinical decision-support system, a diagnostic tool, or a standalone hallucination/error detector.

## Model and benchmark

- Model: `microsoft/biogpt`
- Benchmark: MedMCQA
- Main analysis: 200 questions
- Intervention analysis: 199 evaluable questions

MedMCQA should be obtained from its original public source in accordance with its terms. This repository does not redistribute the full benchmark.

## Repository structure

```text
notebooks/
  01_BioGPT_Attention_Graph.ipynb
  02_Dangling_Centrality.ipynb
  03_Intervention_Faithfulness.ipynb

results/
  overall_prediction_performance.csv
  dangling_centrality_summary.csv
  intervention_faithfulness_summary.csv
  paired_wilcoxon_summary.csv
  roc_auc_summary.csv
  correlation_summary.csv

figures/
  BioGPT_Auditing_Figure.png
  OmegaMH_correct_vs_incorrect.png
  roc_auc_comparison.png

requirements.txt
CITATION.cff
```

## Recommended execution order

1. `01_BioGPT_Attention_Graph.ipynb` — loads BioGPT/MedMCQA, extracts and aggregates self-attention, merges subwords, and constructs/export the sparse token attention graph.
2. `02_Dangling_Centrality.ipynb` — evaluates the undirected structural projection using Dangling Centrality and produces graph-level/node-level outputs.
3. `03_Intervention_Faithfulness.ipynb` — performs the corrected question-stem intervention experiment at 5%, 10%, and 20% deletion levels with `phi_C`, `Omega_MH`, raw attention, and matched random deletion.

The third notebook is included with the executed outputs from the final corrected run used for the intervention results.

## Installation

A Google Colab environment is recommended. For a local Python environment:

```bash
pip install -r requirements.txt
```

The notebooks were designed around `transformers>=4.57,<5` and `datasets>=4.0,<5`. Exact GPU/CUDA availability may affect runtime but should not change the methodological definitions.

## Key reported aggregate results

For the 199 evaluable intervention questions, Dangling Centrality produced the largest descriptive mean AOPC (`0.006924`) compared with raw attention (`0.004473`), `Omega_MH` (`0.003604`), and random deletion (`0.003333`). Paired one-sided Wilcoxon tests comparing `phi_C` with random deletion did not reach the conventional 0.05 significance threshold at 5%, 10%, or 20% deletion (`p = 0.101, 0.084, 0.090`). These results should therefore be interpreted as modest output-sensitivity evidence rather than proof of causal reasoning.

## Reproducibility notes

- Attention heads are averaged within each layer and then averaged across layers.
- BioGPT subwords are merged to readable word-level units before the intervention analysis.
- The sparse directed graph retains the five strongest positive outgoing attention edges per retained token.
- Dangling Centrality uses the undirected, unweighted structural projection and hop-based shortest paths.
- Intervention ranking is restricted to eligible question-stem words; answer-option words are not deleted.
- Random deletion is matched in deletion count and repeated 20 times per question/deletion level.
- Probability drop is measured for the original predicted option.

## Data and code availability

The MedMCQA benchmark is publicly available from its original source. Derived aggregate results used in the manuscript are provided in `results/`. The notebooks in `notebooks/` contain the computational pipeline needed to regenerate the analysis.

## Citation

Citation metadata are provided in `CITATION.cff`. After the GitHub `v1.0.0` release is archived in Zenodo, add the Zenodo DOI to the manuscript's Code Availability and Data Availability statements.

## License

MIT License (when selected during GitHub repository creation).
