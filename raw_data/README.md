# Raw Data Processing

This folder contains scripts for initial data conversion and cleaning of intracranial EEG recordings from the Pacman task.

## Folder Structure

* `./SUBJECT/scripts/`: Subject-specific scripts for data conversion and cleaning
* `./scripts/`: Template scripts for data processing pipeline

## Processing Pipeline

### 1. Data Conversion (`bci_to_fif.ipynb` or `conversion2fif_*.ipynb`)

Converts raw BCI2000 data files to MNE-compatible FIF format:
- Loads signals, states, and parameters from BCI2000 `.dat` files
- Creates MNE info structure with channel names and sampling rate
- Adds photodiode/trial timing as stimulus channel
- Saves as `{subject}_raw_ieeg.fif`

### 2. Behavioral State Extraction (`states_to_csv.ipynb` or `states2csv_*.ipynb`)

Extracts behavioral data from BCI2000 states for task analysis:
- Samples states at 50ms intervals to match behavioral sampling
- Includes: trial number, timing, ghost/user locations, direction, biscuits, attack states, score, lives
- Filters to task period and numbers trials sequentially
- Saves as `{subject}_raw_behave.csv`

### 3. Data Cleaning (`cleaning.ipynb` or `cleaning_*.ipynb`)

Performs artifact removal and preprocessing:
- **Filtering**: Bandpass filter (1-150 Hz), notch filter at 60Hz harmonics
- **Channel rejection**: Removes epileptic channels, noisy channels, non-iEEG channels (EKG, REF, EMPTY)
- **Epoch rejection**: Marks bad epochs from visual inspection
- **Bad trial exclusion**: Removes paused trials and trials without biscuits from behavioral analysis
- Saves cleaned versions: `{subject}_raw_clean_ieeg.fif` and `{subject}_notched_filtered_clean_ieeg.fif`

## Outputs

- **FIF files**: MNE-compatible neural data files for preprocessing pipeline
- **CSV files**: Behavioral state data for alignment with neural analyses
- **Annotations**: Bad epochs and channels marked for exclusion

## Subject-Specific Notes

Cleaning notes documenting epileptic channels, noisy channels, and bad epochs are preserved in individual subject sections below for reference.

[Original subject notes preserved below...]

#### BJH017

hr10

Epileptic channels: BR123, OL123

GR1,2, 3 are first to get spread but not epileptic also B4,5,6

Noisy Channels: 

Bad Epochs: 127, 2; 138-140; 345-347; 547-549; 709-710.5; 820-821; 933.5934; 1014-1015; 1057-1058.5; 1230-1232; 1272-1273; 1327-1330; 1373-1375; 1537-3 (check this); 1671-1672.5;1722-1724; 1786-1788;

#### SLCH002

Epileptic channels: D5, D6; E7; J1, J2, J3

Noisy Channels: I4, I5 (double check, went out half way through)

Bad Epochs: 78-80; 173-175; 441-416; 438-439; 474-476; 564-565; 625-626; 654-656; 690-691; 737-738;  200; 1391-1393

#### LL10

Epileptic Channels: CH25, CH26

Noisy Channels: CH16, CH47, CH28, CH29, CH27, CH24

Bad Epochs: NONE

#### SLCH006

Epileptic/noisy Channels: A1, A10, B4, B5, D6, D7, D8, E9, E10, E11

Notes: No HC electrodes

#### BJH018

Epileptic/noisy Channels: BL1, BL2, BL3, BL4, BL5, CL1, CL2, CL3, CL4, NR1, NR2, NR3, NR4, N5, OR1, OR2, OR3, O$

Notes: No relevant electrodes

#### BJH021

Epileptic/noisy Channels: B1, B2, C1, C2, C3, C4, E8, O1

Notes:

#### LL12

Epileptic/noisy Channels: CH8, CH16, CH17

Notes:


