# UNDER DEVELOPMENT: BEHAV3D Colab

### Overview
BEHAV3D is dynamic immuno-organoid 3D imaging-transcriptomics platform to study tumor death dynamics; immune cell behavior and behavior-guided transcriptomics. On a user friendly way through Google Colab. Data can be located in local or in Google Drive. Under this branch of Github we will post the developing scripts of other future works that might be available.

## Software and Hardware requirements
Google Colab offers a virtual machine that allows for the use of its servers to run without any Software and Hardware requirements. Part of the code will run with CPU and other parts will run with GPU. Particularly, we have two "under development" scripts that use GPU by running DTW with [ParalelDTW](https://colab.research.google.com/drive/1nNPSoUAd6V_zToGsxrhj5CPl4Sz1CJiX) and [SoftDTW](https://colab.research.google.com/drive/1Ln1Hk8kRSMyZP8qCcFyIXZEXR2SDBSGo).

## Set-up
Prior to running any code, you need to set the environment to GPU in Google Colab. You can do this by clicking on "environment control" on the upper left corner of colab, and then to "change type of environment" and click on "T4 GPU". :\
BEHAV3D uses one specific field to customize the analysis:\
