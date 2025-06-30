# SC_FC_Coupling_Task_Personality

## 1. Scope 

This repository contains scripts that were used to conduct the analyses in **"Trait-
Relevance Improves Personality Prediction from Structural-Functional Brain Network Coupling"** coauthored
by Johanna L. Popp, Jonas A. Thiele, Joshua Faskowitz, Caio Seguin, Olaf Sporns and Kirsten Hilger
(doi: will be updated after publication). In brief, we investigated the relationship between the Big Five
Personality traits and structural-functional brain network coupling (SC-FC coupling) operationalized
with similarity and network communication measures during resting state and seven different task
conditions. The scripts found in this repository can be used to replicate all analyses or more generally,
to study the association between intrinsic and task-induced SC-FC coupling and individual differences
(e.g., in personality traits). In case you have questions or trouble with running the scripts, feel free to
reach out under [johanna.popp@uni-wuerzburg.de].

## 2. Data 

For the main sample analysis, data from the S1200 sample of the Human Connectome Project funded
by the National Institute of Health were used (HCP; Van Essen et al., 2013). For the replication of
results, we used data from the PIOP1 and PIOP2 samples collected as a part of the Amsterdam Open
MRI Collection (AOMIC; Snoek et al., 2021). All data analyzed in the current study can be accessed 
online:

HCP: [https://www.humanconnectome.org/study/hcp-young-adult/data-releases/]
AOMIC [PIOP1: https://openneuro.org/datasets/ds002785]
AOMIC PIOP2: [https://openneuro.org/datasets/ds002790]

## 3. Preprocessing

To conduct main analyses in the HCP sample, the minimally preprocessed resting-state fMRI data
from the HCP (Glasser et al., 2013) were used. As additional denoising strategy, nuisance regression
as explained in Parkes et al. (2018; strategy no.6) with 24 head motion parameters, eight mean
signals from white matter and cerebrospinal fluid and four global signals was applied. For task-based
data, basis-set task regressors in addition to the other nuisance regressors were used to remove
mean task-evoked activations (Cole et al., 2019). Most of the fMRI preprocessing steps were
conducted externally, and code can be found here: https://github.com/faskowit/app-fmri-2-mat. To
assess individual structural connectivity, the minimally preprocessed DWI data provided by the HCP
were used and we ran the MRtrix pipeline for DWI processing (Civier et al., 2019; Tournier et al.,
2019; [https://github.com/civier/HCP-dMRI-connectome]). Probabilistic streamline tractography was
employed (Smith et al., 2012) and only streamlines fitting the estimated white matter orientations
from the diffusion image were kept. For the replication analysis, data from the AOMIC PIOP1 and
PIOP2 samples was downloaded in minimally preprocessed form (using fMRIPrep version 1.4.1.;
Esteban et al., 2019) and further processed similarly as in the main sample. Brain networks were
constructed using a multimodal parcellation dividing the brain into 360 nodes (Glasser et al., 2016).

## 4. Structure and Script Description

### 4.1. HCP Data Prep 
For the preparation of data from the HCP sample, the scripts should be run in the following order:

1.	`HCP_MRI_data_import`: Import of all MRI data from folder structure on local machine (SC
 matrices, resting-state fMRI and task fMRI time courses).
2.	`HCP_prepare_behavioral_data`: Import and preparation of HCP behavioral data (Output:
 HCP_behavioral_personality_gscore).
3.	`HCP_prepare_SC_data_with_subcortical`: Preparation of structural connectivity matrices and 
creation of cell that is used for further analyses.
4. `HCP_prepare_FC_resting_state_data_with_subcortical`: Preparation of resting-state
 functional connectivity matrices and creation of a cell that is used for further analyses.
 Included is: a) import of subject ID’s and time course data for all four runs (save in cell) b)
 exclusion of subjects that don’t have all four scans completed c) matching up node order
 according to node order of SC matrices d) computation of functional connectivity matrices
 from time courses e) averaging across all FC matrices for each subject and f) Fisher-z
 transformation of individual mean connectivity matrices.


*Motion Correction*

5.	`HCP_motion_data_import_resting_state`: Import of data for motion correction for 4 resting-
state fMRI scans and creation of respective table.
6.	`HCP_motion_correction_resting_state`: This script is used for motion correction with data
 from framewise displacement (FD). It includes a) definition of resting-state scans that need
 to be excluded b) computation of mean FD values across the remaining scans that are used
 for confound regression. Lastly, FC matrices are excluded based on the motion criteria and
 ultimately saved as final FC matrices and mean FD values in a table used for further analyses.
7.	`HCP_motion_data_import_task_x`: There is one script for each task condition: Import of data
 for motion correction for 2 task-based fMRI scans and creation of respective table.
8.	`HCP_motion_data_correction_task_x`: There is one script for each task condition used for
 motion correction with data from framewise displacement (FD). It includes a) definition of
 task-based scans that need to be excluded b) computation of mean FD values across the
 remaining scans that are used for confound regression. Lastly, FC matrices are excluded
 based on the motion criteria and ultimately saved as final FC matrices and mean FD values
 in a table used for further analyses.
<br>

9.	`HCP_find_subjects_with_complete_data`: Merging of all tables (behavioral data; SC matrices
 and FC matrices) and creation of final tables for subjects that have all data.


*Compute Coupling*

10.	`HCP_compute_coupling_measures_resting_state`: This script is based on a source script from
 Zamani Esfahlani et al. (2022) and was adjusted accordingly. It performs the computation of
 communication and similarity matrices based on SC connectivity matrices and calculates
 coupling measure specific coupling values by correlating regional connectivity profiles of the
 communication/similarity matrix with the respective FC matrix.
11.	`HCP_compute_coupling_measures_task_x`: This script is based on a source script from
 Zamani Esfahlani et al. (2022) and was adjusted accordingly. It calculates coupling measure-
specific coupling values by correlating regional connectivity profiles of the
 communication/similarity matrix (computed with `HCP_compute_coupling_measures_resting_state`)
 with the respective FC matrix.
<br> 

12.	`HCP_split_lockbox_sample`: This script is used to partition the main sample (HCP) into a 
primary main sample (70%) and a lockbox sample (30%) to be able to conduct an initial
replication. It takes the family structure in the HCP into account and makes sure that all 
families are in the same sample to keep them truly independent from one another.

### 4.2. Analysis Scripts 
Please note that some of these scripts were also used to conduct analyses in the paper 
**"Structural-functional brain network coupling during cognitive demand reveals intelligence-relevant
communication strategies (https://doi.org/10.1038/s42003-025-08231-4)"**. Thus, outcome variables
are sometimes still named 'intelligence', even though the personality scores were used in this analysis. 

#### 4.2.1. Scripts for HCP - Main Sample 
In order to perform the analyses conducted in the main sample, the scripts should be run in the 
following order: 

1.	`HCP_region_specific_coupling_all_conditions`: Computation of region-specific SC-FC coupling 
maps (measure that is able to explain the highest variance in FC across all participants most 
frequently). 
2.	`HCP_whole_brain_coupling_plot_across_conditions`: Computation of coupling measure-
specific brain-average coupling values for all eight conditions, taking the mean
 across the four measure-specific coupling values per condition and creating the
 violin plot visualizing condition-specific differences in brain-average SC-FC coupling. 
3.	`HCP_whole_brain_coupling_plot_across_coupling_measures`: Computation of coupling 
measure-specific brain-average coupling values for all eight conditions, taking the mean 
across the eight condition-specific coupling values per coupling measure and creating the 
violin plot visualizing coupling measure-specific differences in brain-average SC-FC coupling. 
4.	`HCP_whole_brain_coupling_all_conditions_correlation_personality`: Computation of coupling 
measure-specific brain-average coupling values for all eight conditions and performance of 
partial correlations with individual personality scores. 
5.	`HCP_internal_cross_validation_basic_NMA_prediction_model`: Conduction of the internal-
cross-validation of the ‘Basic NMA Model’ that is built using two input predictor variables.
The predictor variables are derived from individual’s coupling values and extracted by using 
group-based positive and group-based negative NMA masks. This script contains three parts: 
**Part 1** partitions the sample into five different folds (considering family relations and 
personality trait distribution) and creates positive and negative NMAs for each training 
sample and test sample. **Part 2** uses the NMAs created in part 1 to build multiple linear 
regression models that are then tested for their ability to predict personality scores in the 
test samples. **Part 3** assesses the significance of the prediction with a permutation test.
6.	`HCP_internal_cross_validation_expanded_NMA_prediction_model`: Conduction of the 
internal-cross-validation of the ‘Expanded NMA Model’ that is built using 14 input predictor 
variables (two from each of the seven task conditions). The predictor variables are derived 
from individual’s coupling values and extracted by using group-based positive and group-
based negative NMA masks. This script contains three parts: **Part 1** partitions the sample into 
five different folds (considering family relations and personality distribution) and creates 
positive and negative NMAs for each training fold and test fold. **Part 2** uses the NMAs created 
in part 1 to build multiple linear regression models that are then tested for their ability to 
predict personality scores in the test samples. **Part 3** assesses the significance of the 
prediction with a permutation test.


*Latent NMA Prediction Models (only internal CV)*  
 
This folder contains one script for each trait-specific model combining region-specific SC-FC 
coupling values from trait-relevant or trait-irrelevant tasks to predict individual personality scores. 

7.	`HCP_internal_cross_validation_latent_prediction_model_`: Conduction of the internal-cross-
validation of the ‘Latent NMA Model’ that is built using two input predictor variables. The *
predictor variables are derived from individual’s coupling values and extracted by using 
group-based positive and group-based negative NMA masks. This script contains three parts: 
**Part 1** partitions the sample into five different folds (considering family relations and 
personality distribution) and creates positive and negative NMAs for each training sample 
and test sample. **Part 2** uses the NMAs created in part 1 to build multiple linear regression 
models that are then tested for their ability to predict personality scores in the test samples. 
**Part 3** assesses the significance of the prediction with a permutation test.
<br>

8.	`HCP_external_validation_basic_NMA_prediction_model_in_HCP_lockbox`: Conduction of the 
external validation of the 'Basic NMA Model' that is built using two input predictor variables. 
The predictor variables are derived from individual's coupling values and extracted by using 
group-based positive and negative NMA masks. The script contains three parts: **Part 1** 
partitions the sample into five different folds (considering family relations and personality 
distribution) and creates a positive and negative NMAs for each training sample (4/5 folds) 
and test sample (lockbox sample in this case). **Part 2** uses the NMAs created in part 1 to build 
multiple linear regression models that are then tested for their ability to predict personality 
scores in the test samples (lockbox sample in this case). **Part 3** assesses the significance of 
the prediction with a permutation test.

9.	`HCP_external_validation_expanded_NMA_prediction_model_in_HCP_lockbox`: Conduction 
of the external validation of the ‘Expanded NMA Model’ that is built using 14 input predictor 
variables (two from each of the seven task conditions). The predictor variables are derived 
from individual’s coupling values and extracted by using group-based positive and group-
based negative NMA masks. This script contains three parts: **Part 1** partitions the sample into 
five different folds (considering family relations and personality distribution) and creates 
positive and negative NMAs for each training sample (4/5 folds) and test sample (lockbox 
sample in this case). **Part 2** uses the NMAs created in part 1 to build multiple linear 
regression models that are then tested for their ability to predict personality scores in the 
test sample (lockbox sample in this case). **Part 3** assesses the significance of the prediction 
with a permutation test.

*Test Model Difference Personality vs. Intelligence*

7. `HCP_532_test_significance_model_difference`: This script tests whether there are significant 
differences in prediction model performance between the internally cross-validated Basic NMA 
Model (using coupling data from all conditions separately) and Expanded NMA Model in the main 
sample when predicting intelligence scores vs. predicting personality scores. To do so, the actual 
difference in prediction performance is compared to differences in prediction performances based on 
permuted scores.

#### 4.2.2. Scripts for HCP – Lockbox Sample 
In order to perform the analyses conducted in the lockbox sample, the scripts should be run in the 
following order:

1.	`HCP_lockbox_region_specific_coupling_all_conditions`: Computation of region-specific SC-FC 
coupling (measure that is able to explain the highest variance in FC across all participants 
most frequently).
2.	`HCP_lockbox_whole_brain_coupling_plot_across_conditions`: Computation of coupling 
measure-specific brain-average coupling values for all eight conditions, taking the mean 
across the four measure-specific coupling values per condition and creating the violin plot 
visualizing condition-specific differences in brain-average SC-FC coupling. 
3.	`HCP_lockbox_whole_brain_coupling_plot_across_coupling_measures`: Computation of 
coupling measure-specific brain-average coupling values for all eight conditions, taking 
the mean across the eight condition-specific coupling values per coupling measure and creating 
the violin plot visualizing coupling measure-specific differences in brain-average SC-FC 
coupling. 
4.	`HCP_lockbox_whole_brain_coupling_all_conditions_correlation_personality`: Computation of 
coupling measure-specific brain-average coupling values for all eight conditions and 
performance of partial correlations with individual personality scores. 
5.	`HCP_internal_cross_validation_basic_NMA_prediction_model`: Conduction of the internal-
cross-validation of the ‘Basic NMA Model’ that is built using two input predictor variables. 
The predictor variables are derived from individual’s coupling values and extracted by using 
group-based positive and group-based negative NMA masks. This script contains three parts: 
**Part 1** partitions the sample into five different folds (considering family relations and 
personality distribution) and creates positive and negative NMAs for each training sample 
and test sample. **Part 2** uses the NMAs created in part 1 to build multiple linear regression 
models that are then tested for their ability to predict personality scores in the test samples. 
**Part 3** assesses the significance of the prediction with a permutation test.
6.	`HCP_lockbox_internal_cross_validation_expanded_NMA_prediction_model`: Conduction of 
the internal-cross-validation of the ‘Expanded NMA Model’ that is built using 14 input 
predictor variables (two from each of the seven task conditions). The predictor variables are 
derived from individual’s coupling values and extracted by using group-based positive and 
group-based negative NMA masks. This script contains three parts: **Part 1** partitions the 
sample into five different folds (considering family relations and personality distribution) and 
creates positive and negative NMAs for each training sample and test sample. **Part 2** uses the 
NMAs created in part 1 to build multiple linear regression models that are then tested for 
their ability to predict personality scores in the test samples. **Part 3** assesses the significance 
of the prediction with a permutation test.

### 4.3. Figures

1. `Visualization_distribution_of_personality_scores`: Creation of histograms depicting the distribution 
of personality scores in the main sample (HCP532) and the lockbox sample (HCP232). 
2. `Visualization_HCP_532_NMAs_whole_sample_conscientiousness`: This script builds a positive and 
negative Node-Measure Assignment (NMA) for the complete main sample (HCP532, no cross-
validation) and plots them.
3. `Visualization_prediction_results`: Creation of plots to visualize the results of the prediction 
analyses (bar graphs and matrices).

### 4.4. Functions 

External functions and their licenses can be found in this folder. 

## 5. Software requirements 
+ Matlab version 2021a

## Copyright

Copyright (cc) by Johanna L. Popp 

<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />

Files of **SC_FC_Coupling_Task_Personality** by Johanna L. Popp are licensed under <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

Note that external functions have other licenses that are provided in the `Functions` folder. 
