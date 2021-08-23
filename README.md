# Localizing NP detected spikes and registering raw data

## This repository provides code for localizing the spikes detected in Neuropixels recordings, estimating motion from localization results, and tools for visualizing and evaluating the output of any spike sorter.

### Localization works as follow : 
 - It takes as input the detected spikes and filtered + standardized data
 - Spikes are read from the data and denoised by a Neural Network denoiser. Pre-trained weights are available on github. 
 The Pre-trained NN Denoiser model expects the spikes to be of temporal length 121 and aligned so that their minimum is reached at timestep 42. 
 - Spikes are then localized for each batch of data (for example each second of data), obtained positions and features are stored into the desired repository before being merged to give final results. 
 
Localization code is designed to be fully self-contained. Code to read data and denoise data is written following the YASS pipeline (YASS: Yet Another Spike Sorter applied to large-scale multi-electrode array recordings in primate retina, Lee et al., 2020) and github repository (https://github.com/paninski-lab/yass).
Instructions to train and obtain a new NN-Denoiser can be found on YASS github repository. 


### Motion estimate works as follow : 
 - It takes as input the localizations/amplitudes/spike times/geometry array of spikes.
 - A raster plot is generated from the inputs, and it is destriped/denoised.
 - By default, non-rigid image-based decentralized registration is run, and the
   motion estimate + original raster + registered raster are saved.


### Visualisation : 

The repository contain a script that provides a 3d interactive visualization of the spike train and its localization features. 
Code requires Datoviz (https://github.com/datoviz/datoviz.git, see documentation here https://datoviz.org) 

### Quality Metrics : 

The files in quality_metric show how to compute simple metrics for quantitatively evaluating the quality of registration or the quality of the spike train. 
drift_metrics.py is a script containing code to derive a general correlation score (taken from A New Coefficient of Correlation, Sourav Chatterjee, 2020) between the amplitude (PTP) of units and displacement estimate, firing rate and displacement estimates, and between the first PC of neural activity and displacement. 
The first two scores allow to evaluate the quality of individual units (ideally, a good unit would have low FR and PTP correlation with displacement) and the quality of the recording (which should have low correlation between the first PC of neural activity and displacement). 
