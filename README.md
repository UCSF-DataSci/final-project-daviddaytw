# Neuron Cytoplasm Segmentation Using Stardist and Cellpose for TDP-43 Analysis in FTD

## Team Members

- **David Day**

## Project Overview

This project focuses on segmenting neuron cytoplasm in 3D fluorescence microscopy images to support analysis of the TDP-43 protein implicated in Frontotemporal Dementia (FTD). The goal was to train and adapt two segmentation models—**Stardist** and **Cellpose**—to accurately detect and delineate neuron boundaries, which are critical for downstream morphological and protein distribution analyses.

## Problem Statement

Neuron cytoplasm segmentation is challenging due to:

- Irregular, non-spherical shapes of neurons.
- A small annotated dataset (only 30 3D-labeled images).
- Limitations of pretrained models, which are generally tuned for rounder objects like nuclei.

## Dataset Description

- **Source**: In-house lab-generated 3D fluorescence microscopy data.
- **Annotations**: Manual 3D segmentations of neuron cytoplasm (30 volumes).
- **Input Dimensions**: Each image is a 3D volume with shape `(Z, Y, X)` (e.g., 32×512×512 voxels).
- **Features**: NEUN stained fluorescence intensity.
- **Output**: Binary or labeled mask indicating segmented cytoplasmic regions.

## Tools and Methods

### Segmentation Models

  - [Stardist (3D)](https://github.com/stardist/stardist)
  - [Cellpose (3D)](https://github.com/MouseLand/cellpose)

### Training Strategy

  - Fine-tuned both models on custom neuron data.
  - To combat overfitting on small data, selectively froze backbone layers in the models.

### Frameworks Used

  - Python (3.9)
  - TensorFlow/Keras (for Stardist)
  - PyTorch (for Cellpose)
  - NumPy, SciPy, Scikit-image for preprocessing

## Key Decisions and Trade-offs

- **Freezing Layers**: Due to the limited dataset size, we froze early convolutional layers to retain general feature representations from pretrained weights while fine-tuning higher layers for neuron-specific features.
- **Model Choice**: Tried both Stardist and Cellpose to compare Stardist struggled with non-round geometries, while Cellpose better captured cell boundaries with shape priors but will lost cells.
- **Cut for Time**: Did not implement test-time augmentation or full 3D post-processing due to project time constraints, which may limit segmentation accuracy near borders.

## Issues Overcome

- **Overfitting**: Addressed by freezing layers, aggressive dropout, and using early stopping during training.
- **Memory Constraints**: 3D volumes are large; cropped input to sub-volumes and used data generators.
- **Label Scarcity**: Explored elastic deformations and intensity augmentations to synthetically expand dataset.

## Citations

* **Data**: Private lab dataset
* **Stardist**: Schmidt, U., Weigert, M., Broaddus, C., & Myers, G. (2018). Cell Detection with Star-convex Polygons. *MICCAI*.
* **Cellpose**: Stringer, C., Wang, T., Michaelos, M., & Pachitariu, M. (2021). Cellpose: a generalist algorithm for cellular segmentation. *Nature Methods*.
