# Pacman iEEG Analyses


This repo is used to analyze the intracranial EEG data for the Pacman task. This includes data cleaning, rereferencing, visualization of the signals, and prep for joining with the task behavior. Mostly written in python utilizing MNE.

### Folder Notes

* `raw_data` has scripts for data cleaning as well as notes about the sub-specific cleaning
* `preprocessing` has the subject-specific scripts for iEEG analysis
* `connectivity` has the scripts to calculate coherence metrics and granger causality
* `across_subject_analyses` has the scripts to jointly plot results in MNI space, as well as create averaged time-frequency plots across the different time points



