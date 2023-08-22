# BEHAV3D Colab pipeline
## Overview
BEHAV3D is dynamic immuno-organoid 3D imaging-transcriptomics platform to study tumor death dynamics; immune cell behavior and behavior-guided transcriptomics. On a user friendly way through [Google Colab](https://colab.research.google.com/?hl=es). Data can be located in local or in [Google Drive](https://www.google.com/intl/es_es/drive/).

## What type of data does BEHAV3D work with?
- Any type of multispectral time-lapse 3D (or 2D) imaging data, where objects such as tumor cells or tumor organoids are in co-culture with immune cells of interest.
## How to upload files and what type of files does BEHAV3D accept?
- You need to upload as many files ending with "_Statistics" with the unique ID or name as number of datasets you want to compare
![image](https://github.com/Alvaro-MirandadeLarra/BEHAV3D_Colab/assets/120657569/a86aec55-705e-48bb-87d2-bd8776b813fa)
- Inside each folder uploaded you at least need to have the following statistics in a ".csv" format as shown in the following image:
![image](https://github.com/Alvaro-MirandadeLarra/BEHAV3D_Colab/assets/120657569/7fc0c82e-0eee-43ad-822c-b94133232578)

## What output can BEHAV3D provide?
- Any type of change of cell state that can be detected by a change in fluorescent intensity e.g. cell death, reporter, Ca2+ signalling
- Classification of different types of cell dynamics
- Tumor death dynamics quantification
- Backprojection of behavioral phenotype in Imaris 3D image visualization software
- Correlation between tumor death dynamics and behavioral phenotypes
## How to cite this pipeline
Dekkers JF*, Alieva M*, Cleven A, Keramati F, Wezenaar AKL, van Vliet EJ, Puschhof J, Brazda P, Johanna I, Meringa AD, Rebel HG, Buchholz MB, Barrera Román M, Zeeman AL, de Blank S, Fasci D, Geurts MH, Cornel AM, Driehuis E, Millen R, Straetemans T, Nicolasen MJT, Aarts-Riemens T, Ariese HCR, Johnson HR, van Ineveld RL, Karaiskaki F, Kopper O, Bar-Ephraim YE, Kretzschmar K, Eggermont AMM, Nierkens S, Wehrens EJ, Stunnenberg HG, Clevers H, Kuball J, Sebestyen Z, Rios AC. **Uncovering the mode of action of engineered T cells in patient cancer organoids**. * *equal contibution* Nat Biotechnol. 2023 Jan https://doi.org/10.1038/s41587-022-01397-w
## Software and Hardware requirements
Google Colab offers a virtual machine that allows for the use of its servers to run without any Software and Hardware requirements. Part of the code will run with CPU and other parts will run with GPU.

## Input data
The current version of the pipeline works with objects (cells or organoids) time-lapse statistics that are aquired by tracking these objects in a commercially available software (Imaris, Oxford Instruments).
However any type of time-lapse data can be processed with the pipeline, including measruements extract from MTrackJ (Fiji) or others. Main feature that is needed are coordinates for the objects and a common ID for the same object that is tracked over time. Aditional statistics describing the cell behavior such as speed, displacement are calculated by Imaris, however they can also be calculate by pre-processing algorithms from the cell coordinates. Statistics related to the expression of markers of interest (e.g live-dead cell dye) should be included to study the dynamic expression of these overtime. For statistics related to distance to organoids, use the *min_intensity in ch X* (corresponding to the channel number created by the Distance transformation Xtension. Rename it to be called *dist_org*.

## Dataset example
In this repository we provide example datasets consisting of a multispectral time-lapse 3D imaging dataset originated from a co-culture of engeneered T cells and Tumor derived organoids from the BEHAV3D [original paper](https://www.nature.com/articles/s41587-022-01397-w). Multispectral imaging allows to identify: Live/dead T cells; Live/Dead organoids. For downstream analysis of organoids: Either individual tumor derived organoids are tracked overtime or the total organoid volume per well is tracked. For each generated object we acquire information on the dead cell dye intensity and position and volume of individual organoids. For downstream analysis of T cell: T cells are tracked overtime. For each Tracked T cell object we aquire, position per timepoint, speed, square displacement, distance to an organoid, dead dye intensity, major and minor axis length (used in some downstream analysis).

## Set-up
BEHAV3D uses one specific field to customize the analysis:\

### **Experimental metadata template**
To correctly import data for BEHAV3D, it is required to fill in a .tsv that contains information per experiment performed, requiring information on:
- Experiment basename (same name as your folder where the surfaces exported statistics are stored without "_Statistics", that is created by default)
- organoid_line (tumor or organoid line used in that particular well)
- tcell_line (name of the T cell concept and subpopulation if different populations were labelled)
- exp_nr
- well
- date
- dead_dye_channel (Channel # that contains the dead dye intensities)
- organoid_distance_channel (Channel # that contains the distance transformation, if distance transformation step was not used, write any channel number that you have)
- tcell_contact_threshold (threshold of distance to other tcells to be considered touching, usually set to average cell diameter (10um), change if cells are of different size )
- tcell_dead_dye_threshold (threshold to consider an tcell "dead", can be visually estimates from the Imaris file based on mean intentisity of ch "dead cells")
- (optional)tcell_stats_folder (path to folder with tcell track statistics)
- organoid_contact_threshold (threshold of distance to organoid to be considered touching,estimate from the Imaris file based on min distance to ch "distance transformation" in T cell touching the organoid. Note that while this value is usually fixed in the same experiment, it might change if image resolution is different or between Imaris versions)
- organoid_dead_dye_threshold (threshold to consider an organoid "dead", same as for tcell_dead_dye_threshold)
- (optional)organoid_stats_folder (path to folder with organoid track statistics)
- tumor_name (name of the tumor/organoids surfaces that were created with imaris, only required if you Object-Object statistics to import the distance to tumor data)
- Object_distance (TRUE if Object-Object statistics were used to import the data related to T cell distance to tumor cells/ FALSE if distance transformation was for this step )

For an example see: [...BEHAV3D/configs/metadata_template.tsv](https://github.com/Alvaro-MirandadeLarra/BEHAV3D_Colab/blob/main/configs/metadata_template.tsv)\

## Demo

You can run BEHAV3D on demo data to see examples of the results. This will take <30 minutes\
\
There are 2 demos:
- tcell_demo    [For 'tcell_dynamics_classification'](https://github.com/Alvaro-MirandadeLarra/BEHAV3D_Colab/tree/main/demos/tcell_demo)
- organoid_demo [For 'organoid_death_dynamics'](https://github.com/Alvaro-MirandadeLarra/BEHAV3D_Colab/tree/main/demos/organoid_demo)

## Modules
### (1) Organoids death dynamics module

This module examines the organoid death over time (individual organoids and per well statistics)

***To run from Google Colab:***

**>Step 1** For demo mode run [organoid_death_dynamics script](https://colab.research.google.com/drive/1oA22RHC9Lk8_zlyGiFyr52_INAKT7QtR)


***Output_files***

All organoid death dynamics unfiltered
- Full_well_death_dynamics.pdf  (Mean of dead dye intensity over the full well)
- Full_well_death_dynamics.rds
- Full_individual_orgs_death_dynamics.pdf   (Mean of dead dye intensity over per organod track in each well)
- Full_individual_orgs_death_dynamics.rds
- Full_percentage_dead_org_over_time.pdf    (Percentage of overall organoid death occuring over time)
- Full_percentage_dead_org_over_time.rds

Filtered organoid death dynamics based on BEHAV3D config
- Exp_well_death_dynamics.pdf
- Exp_well_death_dynamics.rds
- Exp_individual_orgs_death_dynamics.pdf
- Exp_individual_orgs_death_dynamics.rds
- Exp_percentage_dead_org_over_time.pdf
- Exp_percentage_dead_org_over_time.rds

### (2) T cell behavior classification module

This module examines the tcell dynamics and either performs clustering (no randomforest supplied) or classifcation (randomforest supplied). To generate the [Random Forest](https://colab.research.google.com/drive/1mvMBMIDWYDIVDCgVJfjTeAww7aKr7g0b).

***To run from Google Colab***\
**>Step 2** For demo run  [predict_tcell_behavior](https://colab.research.google.com/drive/1v6jKiE9nE8yFwgWKiRlINKyO5YCLFQHJ)

***Output_files***

output rds:
- raw_tcell_track_data.rds (Combined raw track data for all experiments)
- processed_tcell_track_data.rds (Combined processed track data for all experiments; Added contact, death etc.)
- behavioral_reference_map.rds   (Output of the t_cell behavior that can be used to train your own randomForest)
- classified_tcell_track_data.rds (Combined classified track data - Either randomForest or clustering - for all experiments)
- classified_tcell_track_data_summary.rds (Summary of classified track data - Either randomForest or clustering- for all experiments)
- cluster_perc_tcell_track_data.rds (Cluster percentages for the track data - used for creation of RF_ClassProp_WellvsCelltype.pdf)
- (If no randomForest) tcell_track_features.rds (Track features used to create Cluster_heatmap.pdf)

output pdf:
- (If randomForest supplied) RF_ClassProp_WellvsCelltype.pdf (Proportion of each randomForest predetermined cluster for all experiments)
- (If no randomForest) Umap_unclustered.pdf
- (If no randomForest) Umap_clustered.pdf
- (If no randomForest) Cluster_heatmap.pdf (Heatmap of track features for created clusters)
- (If no randomForest) umap_cluster_percentage_bars_separate.pdf (Proportion of each cluster for each separate experiment)
- (If no randomForest) umap_cluster_percentage_bars_combined.pdf (Proportion of each cluster for combiend experiments - based on organoid_lines and tcell_lines)

quality control:
- NrCellTracks_filtering_perExp.pdf (Shows the number of tracks remaining after each filtering step, shown per experiment)
- NrCellTracks_filtering_perFilt.pdf    (Shows the number of tracks remaining after each filtering step, shown per filtering step)
- TouchingvsNontouching_distribution.pdf    (Shows the number of Touching vs. Non-Touching T cells based on the defined "tcell_contact_threshold")
- DeadDye_distribution.pdf (Shows the distribution of mean dead dye intensity in Tcells per experiment, can be used to set correct "tcell_dead_dye_threshold" for dead cells)

### (3) T cell behavioral classification backprojection module

This module allows you to export the classified T cell tracks to visualize them in Imaris.

***To run from Google Colab***\
**>Step 3** For demo run  the [backprojection_tcell_classification](https://colab.research.google.com/drive/10icFW3jdo3-mbC_XfUTXXSTeRTTc0yU1) script to save the behavioral classification for each processed T cell. This can then be uploaded in Imaris via the tracks search module.

### **Making changes to the notebook**\n",
        "\n",
        "<font size = 4>**You can make a copy** of the notebook and save it to your Google Drive. To do this click file -> save a copy in drive.\n",
        "\n",
        "<font size = 4>To **edit a cell**, double click on the text. This will show you either the source code (in code cells) or the source text (in text cells).\n",
        "You can use the `#`-mark in code cells to comment out parts of the code. This allows you to keep the original code piece in the cell as a comment."
