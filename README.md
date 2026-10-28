# Gene expression and dendrite morphology in mouse motor cortex (Patch-seq)

Which genes change expression together with dendrite shape, within cell types?
Data: Patch-seq of adult mouse motor cortex, Scala et al. 2021 (605 cells after QC, 603 with usable dendrite morphology).

## Approach
1. **Cells and QC** (01): exon counts, cell filtering on genes detected, log2(CPM + 1).
2. **Morphology** (02): 19 dendrite/soma/stem features shared by excitatory and inhibitory cells, Yeo-Johnson transform, redundant features dropped, family means removed, PCA to 4 axes (parallel analysis).
3. **Gene models** (03): per gene and per RNA family, expression ~ axis + QC covariates + batch, mouse-clustered SEs; fixed-effect meta-analysis across 7 families; BH FDR. A naive pooled analysis is shown as a control.
4. **Validation** (04): within-family permutation null (500 shuffles) and empirical FDR. 
5. **Function** (05): gene-by-feature heatmap, example genes, GSEA prerank on GO Biological Process, replication of the top terms across families.

## How to run
1. Clone this repo, then clone the data into it:
   `git clone https://github.com/berenslab/mini-atlas`
2. Install the packages: `conda env create -f environment.yml` (or `pip install -r requirements.txt`, Python 3.12).
3. Run the notebooks in order, 01 to 05. Random seed: 12345. Paths are relative to the repo folder.

| Notebook | Main outputs |
|---|---|
| `01_explore_and_prepare_data.ipynb` | `results/QC/` (covariates, QC cell counts, normalised expression) |
| `02_morphology_features.ipynb` | `results/morphology/` (axes, loadings), `results/figures/02_*.png` |
| `03_gene_morphology_models.ipynb` | `results/models/meta_results.csv`, `hits_per_family.csv`, `naive_pooled_results.csv` |
| `04_statistical_validation.ipynb` | `results/models/meta_results_empirical.csv`, `meta_feature_z.csv`, `power_spike_in.csv`, QQ figure |
| `05_function_and_figures.ipynb` | `results/models/gsea_prerank_GO_BP.csv`, figures |

## Versions used
Python 3.12.5, numpy 2.0.2, pandas 2.2.3, scipy 1.14.1, scikit-learn 1.5.2, matplotlib 3.9.2, seaborn 0.13.2, statsmodels 0.14.4, gseapy 1.3.1.

## References
- Scala F, Kobak D, et al. Phenotypic variation of transcriptomic cell types in mouse motor cortex. Nature 598, 144-150 (2021).
- Code and data: https://github.com/berenslab/mini-atlas.
