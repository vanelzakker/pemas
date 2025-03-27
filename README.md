# PEMAS Pipeline
Pipeline for Extricating Movement from Auricular Stimules (PEMAS). This repository contains code and scripts for analyzing 7T fMRI data acquired during a Respiratory-gated Auricular Vagal Afferent Nerve Stimulation (RAVANS) task and resting-state scans. The pipeline uses tools from Slocco's Laboratory, fMRIPrep, and FSL to preprocess data, regress physiological noise, and perform statistical analysis.

## Data and Preprocessing

* **Data Source:** 7T fMRI data collected during RAVANS task and resting-state. Physiological recordings (heartbeat and respiration) were acquired for noise regression.
* **Pipeline Foundation:** The code is based on a pipeline provided by Slocco's Laboratory (Roberta Slocco, and others).
* **Physiological Noise Correction:**
    * `phycorr` (RETROICOR and R-DECO) will be used to regress physiological noise.
* **fMRIPrep:**
    * fMRIPrep will be used for comprehensive preprocessing, including:
        * Motion correction using aCompCor.
        * Slice-timing correction.
        * Spatial normalization to MNI and T1w space.
        * FreeSurfer `recon-all` for surface reconstruction (if enabled).
        * Generation of motion regressors from aCompCor.
* **Output:** Preprocessed BOLD data and motion parameter tables from aCompCor.

## Analysis Steps

1.  **Brainstem and Medulla Mask Extraction:**
    * `1_apply_brainmask_afterfmreiprep.sh` extracts brainstem and medulla masks from the fMRIPrep output.
2.  **Motion Regressor Preparation:**
    * `2_FD_FEAT_headmotion.sh` processes the aCompCor motion parameter tables to generate text files containing regressors for FSL's FEAT.
3.  **Stimulus Extraction:**
    * `stem_trigger_newvariables_final_final.m` extracts stimulus timing information for FEAT input.
4.  **FEAT First-Level Analysis:**
    * FSL's FEAT is used for first-level analysis, incorporating the preprocessed BOLD data, motion regressors, and stimulus timing.
5.  **FEAT Output Fusion:**
    * `GUTBRAIN_FLOBS_sumstat.m` fuses the parameter estimates (PEs) from the FEAT first-level analysis.
6.  **Second-Level Analysis:**
    * `3_fslmaths.sh` calculates the difference between task and resting-state contrasts.
    * `5_fslmerge.sh` merges the resulting contrast parameter estimate (COPE) images.
    * **Randomise:** FSL's `randomise` is then used for non-parametric permutation testing to perform statistical inference. Randomise allows for robust statistical testing, especially with data that may not meet the assumptions of parametric tests, by creating a null distribution through permutations of the data. This provides a way to control for multiple comparisons and determine the significance of observed effects.

## Requirements

* FSL
* fMRIPrep
* MATLAB
* phycorr
* Bash

## Usage

Follow the numbered scripts and MATLAB functions in sequence to execute the analysis pipeline. Ensure that the necessary software and dependencies are installed and configured correctly.
