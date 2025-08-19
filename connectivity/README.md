# Connectivity Analyses

This folder contains scripts for computing functional connectivity measures between brain regions during the Pacman task.

## Folder Structure

* `./scripts/granger/`: Granger causality analysis scripts
* `./scripts/turnaround_coherence/`: Coherence and phase coupling analysis scripts
* `./scripts/turnaround_coherence/compute_scripts/`: Subject-specific connectivity computation scripts

## Analysis Types

### 1. Coherence Analysis (`turnaround_coherence/`)

**Main Functions** (`connectivity_functions.py`):
- `compute_coherence()`: Calculates spectral connectivity using MNE-connectivity
  - Methods: Imaginary coherence (imcoh), phase-phase coupling (ppc), weighted phase lag index (wpli2_debiased)
  - Uses Morlet wavelets for time-frequency decomposition
- `compute_epoch_coherence()`: Trial-by-trial connectivity measures
- `shuffle_epochs()`: Creates permuted null distributions for statistical testing

**Computation Pipeline**:
1. Subject-specific scripts (`compute_connectivity.py`, `{subject}_compute_connectivity.py`)
   - Load preprocessed epochs from last_away timepoint
   - Define region pairs of interest (e.g., OFC-cingulate, amygdala-OFC, hippocampus-amygdala)
   - Compute connectivity metrics across frequency bands (1-150 Hz)
   - Save results as pickle files

2. Permutation Testing (`compute_true_connectivity.py`)
   - Generate null distributions through trial shuffling
   - Statistical significance assessment

3. Results Aggregation (`concatenate_connectivity_results.ipynb`, `concat_perms.ipynb`)
   - Combine results across subjects
   - Apply multiple comparisons correction

### 2. Granger Causality Analysis (`granger/`)

**Analysis Pipeline**:

1. **True Granger Computation** (`compute_true_granger.py`):
   - Loads significant theta-band electrode pairs from coherence analysis
   - Filters epochs by trial conditions (attack/escape/ghost/no-ghost)
   - Computes time-domain Granger causality between ROI pairs
   - Focus on directional connectivity patterns

2. **Permutation Testing** (`compute_permuted_granger*.py`):
   - Multiple scripts for parallel computation across subjects
   - Creates shuffled null distributions for statistical validation
   - HPC folder contains scripts optimized for cluster computing

3. **Statistical Analysis** (`compute_granger_pvalues.py`):
   - Compares true Granger values to permuted distributions
   - Applies FDR correction for multiple comparisons
   - Identifies significant directional connections

## Key Parameters

- **Frequency Range**: 1-150 Hz (log-spaced)
- **Time-Frequency Method**: Morlet wavelets with log-spaced cycles
- **ROI Pairs Analyzed**:
  - OFC ↔ Cingulate
  - OFC ↔ MFG (middle frontal gyrus)
  - Amygdala ↔ OFC/Cingulate/MFG
  - Hippocampus ↔ Amygdala/OFC/Cingulate/MFG

## Outputs

- **Connectivity matrices**: Saved as pickle/h5 files per subject and condition
- **Statistical results**: CSV files with significant connections and p-values
- **Permutation distributions**: Stored for reproducible statistical testing
