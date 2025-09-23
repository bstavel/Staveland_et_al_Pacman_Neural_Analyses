# Pacman iEEG Analyses

This repository contains the complete analysis pipeline for intracranial EEG data collected during a Pacman spatial navigation task. The analyses examine neural dynamics during threat avoidance, reward collection, and strategic decision-making behaviors.

## Overview

The pipeline processes iEEG recordings from multiple subjects, computing time-frequency representations, functional connectivity measures, and group-level statistics. All analyses are implemented in Python using MNE-Python and associated neuroimaging packages.

## Repository Structure

### 1. `raw_data/`
**Initial data processing and artifact removal**
- Converts BCI2000 recordings to MNE-compatible FIF format
- Extracts behavioral states at 50ms resolution for neural-behavioral alignment  
- Performs artifact rejection (epileptic channels, noise removal, bad epoch marking)
- Outputs: Cleaned FIF files and behavioral CSV files for downstream analysis

### 2. `preprocessing/`
**Signal processing and time-frequency decomposition**
- Applies bipolar rereferencing to improve spatial specificity
- Computes time-frequency representations using Morlet wavelets (1-150 Hz)
- Analyzes 6 key behavioral events: trial onset, first move, first reward, escape initiation, ghost attacks, trial end
- Extracts band-specific power (delta, theta, alpha, beta, gamma, HFA) for statistical modeling
- Outputs: Event-locked TFRs and power timeseries per ROI

### 3. `connectivity/`
**Functional connectivity between brain regions**
- Calculates coherence metrics (imaginary coherence, phase-phase coupling, weighted phase lag index)
- Computes directional connectivity using Granger causality
- Analyzes key circuits: hippocampus-amygdala, amygdala-OFC, OFC-cingulate, and frontal networks
- Implements permutation testing for statistical validation
- Outputs: Connectivity matrices and significant connection maps

### 4. `across_subject_analyses/`
**Group-level statistics and visualization**
- Aggregates time-frequency data across subjects by anatomical ROI
- Creates MNI-space visualizations of electrode coverage and results
- Identifies consistent patterns across the subject population
- Generates publication-ready figures and statistical summaries
- Outputs: Grand average TFRs, statistical cluster maps, and group visualizations

### Run Time
For one patient with an average number of electrodes, processing all steps, including the construction of null distributions, should be under two weeks.

## Installation

### Requirements

All required packages are listed in `requirements.txt`. To install dependencies:

```bash
pip install -r requirements.txt
```

**Note:** The `BCI2kReader` package may require manual installation from its source repository if not available via PyPI.

### Key Dependencies

- **MNE-Python** (>=1.0.0): Core iEEG/EEG analysis framework
- **MNE-Connectivity** (>=0.3.0): Functional connectivity metrics (coherence, Granger causality)
- **NumPy/SciPy**: Numerical computing and signal processing
- **Pandas**: Data manipulation and behavioral data management
- **Matplotlib/Seaborn**: Visualization and statistical plotting
- **neurodsp** (>=2.1.0): Neural digital signal processing utilities
- **FOOOF** (>=1.0.0): Spectral parameterization for oscillation analysis
- **statsmodels** (>=0.12.0): Statistical modeling and multiple comparisons correction
- **mat73/h5io**: File I/O for MATLAB and HDF5 formats
- **BCI2kReader**: Reading BCI2000 data format

### Install time (for repo)
Under 5 minutes, assuming main dependencies are already installed

## Data Flow

```
BCI2000 Raw Data → raw_data/ → Cleaned FIF Files
                                      ↓
                              preprocessing/ → TFRs & Power
                                      ↓
                    connectivity/ ←───┘
                           ↓
                 across_subject_analyses/ → Group Results
```



