# Preprocessing README

This folder does all of the preprocessing and some intial subject-specific analyses. 

* `inputs` are `.fif` files that are created in the `./raw_data` folders
* `output` are time-frequency plots and frequency band specific csvs that are used to create figures in `across_subject_analyses` or for Bayesian Linear Mixed Effects models in the corresponding behavioral repo.

## Folder Structures

Folder Structure is as follows: 
* `./SUBJECT/`
   *  `./SUBJECT/ieeg`: preprocessed fif files and TFRS
   *  `./SUBJECT/scripts`: Python Notebooks for preprocessing and computing TFRs for regions of interest at each timepoint
*  `./scripts/`: Has helper scripts, script templates for preprocessing and time-point specific TFR calculation, export to csvs, dictionary match electrodes to regions, etc

## Steps

### Bipolar Rereferencing (`preprocessing.ipynb`)

Pretty self explanatory.

### Individual Subject Time-locking Analyses (`trial_onset.ipynb`, `first_move.ipynb`, `first_dot.ipynb`, `last_away.ipynb`, `ghost_attack.ipynb`, `trial_end.ipynb`)

I then calculate the TFRs based on 6 different events. These scripts generally follow the same format, but I seperate some of the TFRs based on different conditions as is relevant to the specific event. 

I  use Morlet wavelets to calculate the time-frequency representation for each time period of interest from 1-150Hz, using the function mne.time_frequency.tfr_morlet. This will be the basis for all of power calcualtions, which means, for example, I will take the 3-8hz frequencies from the TFR and average them to gether to get Theta power, etc. I am using a log-spaced freqs vector, as well as a log-spaced vector for the number of cycles for each frequency band. Specifically,

```
# Defintion of Frequencies and Number of Cycles
freqs = np.logspace(start = np.log10(1), stop = np.log10(150), num = 80, base = 10, endpoint = True)
n_cycles = np.logspace(np.log10(2), np.log10(30), base = 10, num = 80)

# formulas to check bandwidth and time bin
band_width = (freqs / n_cycles) * 2
time_bin = n_cycles / freqs / np.pi

```

## Baselining

For baselining, I am currently log-transforming the TFR, and then z-scoring within each trial, channel, and frequnecy band.

### Freq Power Csvs

Along with calculating the TFR I also average frequency power within the given frequency bands and save out to cvs. Here is the information relevant frequency-specific information.


|  Freq | Lower  | Upper  | Step  |  Subband |  Subband Info |
|-------|--------|--------|-------|----------|---------------|
|  Delta | 1     |  3     |  np.floor(sfreq/(2*4)) |    No     |  ~ |
|  Theta |  3    |  8     |  np.floor(sfreq/(5*4)) |    No     |  ~ |
|  Alpha  | 8    | 13     |  np.floor(sfreq/(11*4)) |    No     |  ~ |
|  Beta | 13     | 30     |  np.floor(sfreq/(22*4)) |    No     |  ~ |
| Gamma  | 30    | 70     |  np.floor(sfreq/(50*4)) |    yes     |  (30, 40), (35, 45), (40, 50), (45, 55), (50, 60), (55, 65), (60, 70) |
| HFA  |  70     | 150    |  np.floor(sfreq/(110*4)) |    yes     | (70, 90), (80, 100), (90, 110), (100, 120), (110, 130), (120, 140), (130, 150) |


The step is with what frequency I save out the power information to a csv. Higher frequency for higher bands. I chose 4 samples per oscillation, and picked a middle frequency within the band to determine the frequency. 

### Event tables

I use R to create tables of events that I use to create the MNE epoch objects. The scripts live here: https://github.com/bstavel/pacman_behavior/blob/main/R/create_timelock_event_tables.R
Here are some small but tricky points

* When merging the tables as metadata in the timelocking scripts, merge the metadata in before you remove bad epochs. This means that I do not filter out bad trials in the R scripts.
* Also, in the R scripts we will save the events with the `neural_trial_numeric` variable so it already aligns with python 0 indexing, cuz it is confusing to change it other same variable/know if I already changed it before.

