# Preprocessing

This folder contains scripts for signal preprocessing and time-frequency analysis of intracranial EEG data.

## Folder Structure

* `./SUBJECT/ieeg/`: Preprocessed FIF files and time-frequency representations (TFRs)
* `./SUBJECT/scripts/`: Subject-specific preprocessing and TFR computation notebooks
* `./scripts/`: Template scripts, helper functions, and ROI definitions
  - `preproc_functions.py`: Core preprocessing utilities
  - `roi.py`: Electrode-to-region mapping dictionary
  - `export_iti_baselined_csvs.ipynb`: Frequency band power extraction

## Processing Pipeline

### 1. Bipolar Rereferencing (`preprocessing.ipynb`)

Converts monopolar recordings to bipolar montage to reduce common noise and improve spatial specificity.

### 2. Time-Frequency Analysis

**Event-Locked Scripts** (6 behavioral timepoints):
- `trial_onset.ipynb`: Trial start analysis
- `first_move.ipynb`: Initial movement detection
- `first_dot.ipynb`: First reward collection
- `last_away.ipynb`: Escape behavior initiation
- `ghost_attack.ipynb`: Threat encounter responses
- `trial_end.ipynb`: Trial completion analysis

**TFR Computation**:
- Method: Morlet wavelets via `mne.time_frequency.tfr_morlet`
- Frequencies: 1-150 Hz (80 log-spaced frequencies)
- Cycles: 2-30 (log-spaced to balance temporal/frequency resolution)

```python
freqs = np.logspace(np.log10(1), np.log10(150), num=80)
n_cycles = np.logspace(np.log10(2), np.log10(30), num=80)
```

### 3. Baseline Correction

- Log-transform TFR data
- Z-score within each trial, channel, and frequency band
- Preserves trial-specific dynamics while normalizing across conditions

### 4. Frequency Band Power Extraction

**Band Definitions and Sampling**:

| Band | Range (Hz) | Sampling Step | Subbands |
|------|------------|---------------|----------|
| Delta | 1-3 | `sfreq/(2*4)` | No |
| Theta | 3-8 | `sfreq/(5*4)` | No |
| Alpha | 8-13 | `sfreq/(11*4)` | No |
| Beta | 13-30 | `sfreq/(22*4)` | No |
| Gamma | 30-70 | `sfreq/(50*4)` | Yes: 10Hz windows (30-40, 35-45, ..., 60-70) |
| HFA | 70-150 | `sfreq/(110*4)` | Yes: 20Hz windows (70-90, 80-100, ..., 130-150) |

Sampling rate determined by 4 samples per oscillation at band center frequency.

## Event Table Generation

Behavioral events aligned with neural data using R scripts:
- Source: https://github.com/bstavel/pacman_behavior/blob/main/R/create_timelock_event_tables.R
- Uses `neural_trial_numeric` for Python 0-indexing compatibility
- Metadata merged before bad epoch removal to maintain trial alignment

## Outputs

- **TFR files**: Time-frequency representations per subject/event (`.h5` format)
- **CSV exports**: Band-specific power for statistical modeling
- **Preprocessed epochs**: Cleaned, rereferenced data ready for connectivity analysis

