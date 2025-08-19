# Across-Subject Analyses

This folder contains scripts for group-level analyses, combining data across all subjects to identify consistent patterns and generate publication figures.

## Folder Structure

* `./scripts/`: Python scripts and notebooks for group analyses
* `./scripts/anatomy/`: Brain visualization and electrode plotting scripts
* `./ieeg/`: Output directory for group-level results

## Analysis Components

### 1. Anatomical Visualization (`anatomy/`)

**Electrode Plotting Scripts**:
- `plot_all_elecs_on_brain.ipynb`: Plots all electrodes across subjects on MNI brain
- `plot_all_elecs_on_brain_by_subject.ipynb`: Subject-specific electrode visualization
- `plot_all_elecs_on_brain_mfg_group.ipynb`: Focused visualization for middle frontal gyrus electrodes

### 2. Time-Frequency Averaging (`average_tfr_functions.py`)

**Key Functions**:
- `get_roi_elec_lists()`: Identifies electrodes belonging to specific ROIs
- `calculate_trial_onset_average()`: Averages TFRs across subjects for trial onset
  - Handles different sampling rates across subjects
  - Supports subregion selection within ROIs
  - Baseline correction window: -1 to 5 seconds

**Event-Specific Averaging Scripts**:
- `trial_onset_avg_tfrs.py`: Group averaging for trial start
- `first_move_avg_tfrs.py`: Averaging locked to first movement
- `ghost_attack_tfrs.py`: Averaging during ghost attack events

### 3. Group-Level Notebooks

**Statistical Analyses**:
- `trial_onset_allsubs.ipynb`: Group analysis of trial onset responses
- `trial_end_allsubs.ipynb`: Group analysis of trial completion
- `last_away_allsubs.ipynb`: Analysis of escape behavior patterns
- `last_away_allsubs_new_subs.ipynb`: Updated analysis with additional subjects

**ROI Analysis**:
- `count_subject_roi.ipynb`: Quantifies electrode coverage per ROI across subjects

### 4. Batch Processing

- `batch_tfr_notebooks.sh`: Shell script for parallel processing of TFR computations across subjects

## Processing Pipeline

1. **Load Individual TFRs**: Read preprocessed time-frequency data from each subject
2. **ROI Selection**: Extract electrodes corresponding to anatomical regions of interest
3. **Averaging**: Compute grand averages across subjects, handling missing data
4. **Statistical Testing**: Identify significant time-frequency clusters
5. **Visualization**: Generate publication-ready figures in MNI space

## Key Parameters

- **Baseline Period**: Variable by event type, typically -1 to 0 seconds pre-event
- **Frequency Bands**:
  - Delta (1-3 Hz)
  - Theta (3-8 Hz)
  - Alpha (8-13 Hz)
  - Beta (13-30 Hz)
  - Gamma (30-70 Hz)
  - High Frequency Activity (70-150 Hz)

## Outputs

- **Group TFR plots**: Average spectrograms per ROI and condition
- **Statistical maps**: Significant clusters across time-frequency space
- **Electrode coverage maps**: Visualization of recording sites in MNI coordinates
- **CSV exports**: Summary statistics for mixed-effects modeling
