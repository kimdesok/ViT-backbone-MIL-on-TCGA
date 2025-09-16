# Training a ViT backbone MIL on TCGA-BRCA Dataset

>* A ViT backbone MIL model has been trained on TCGA-BRCA dataset and its performance was compared to the End to End model backbone CNN models' performance.

Aims:

1) Acquisition and preprocessing of histopathology image datasets,

2) Exploratory Data Analysis (EDA), and

3) Training experiments with Multiple Instance Learning (MIL) models based on transformer backbones such as ViT.

4) Design of inference services for cancer diagnosis assistance based on pathology image analysis.

5) Development and operation of an NPU-based prototype production platform for preliminary research.

Tasks: Dataset construction, model training experiments, inference service design, Streamlit-based demo coding, and REST API framework-based prototype development for cancer diagnosis inference services.

* Training datasets were converted to TFRecord format. Currently, we have prepared TCGA-BRCA, PatchCamelyon, and Camelyon16 datasets; Camelyon17 will be added later this year.

* Available storage is limited to 2TB, insufficient for holding multiple datasets simultaneously. Downloading and preprocessing each dataset requires more than five days, highlighting the need for storage expansion to at least 5TB.

* To benchmark ViT-MIL, we additionally trained CNN-backbone MIL models using ResNet50, VGG19, and Inception.

* All MIL models have been trained on TCGA-BRCA; Camelyon16 dataset analysis is ongoing, and PatchCamelyon will be used for further training.

## EDA on TCGA-BRCA Dataset
<img width="536" height="280" alt="image" src="https://github.com/user-attachments/assets/ed9323a6-0ccd-4d3b-aeab-72b6d4bb5f80" />
Exploratory Data Analysis (EDA) and Visualization

Camelyon16 dataset provides pixel-level annotations (see Fig. 1).

Experiments were conducted on preprocessing and filtering slides to exclude empty glass regions, keeping only tissue patches (see Fig. 2).

<img width="321" height="412" alt="image" src="https://github.com/user-attachments/assets/56aad0e5-f049-463e-9659-c9814fbfba50" />


SVS-format WSI files were converted to TFRecord format.

Localization of metastatic cancer cells in lymph node tissue was visualized (see Fig. 3). For example:

Panel A: metastatic tissue requires 1,077 patches of 224×224 size,
Panel B: only 2 patches of tissue cells.

<img width="546" height="470" alt="image" src="https://github.com/user-attachments/assets/f0dd4956-558f-4520-9522-0c14202d6757" />

This illustrates the challenge of accurate WSI-level classification using MIL models.

## Performance evaluation on TCGA-BRCA test set

CNN-MIL models generally achieve >90% AUC,

ViT-MIL achieves >95% AUC (see Table 1).

📊 Table 1. Comparison of CNN-backbone MIL vs. ViT-backbone MIL models on TCGA-BRCA

<img width="583" height="101" alt="image" src="https://github.com/user-attachments/assets/4f23fe62-787f-42ea-b283-46a0a9bf1075" />

>* CNN-only models (without MIL) could not be trained due to OOM (Out-Of-Memory) errors on GPUs with 160GB memory (A100 x 2 provided by Elicer group, Seoul, s. Korea).

Inference Service Development(Under Construction)

Since training datasets consist of Whole Slide Images (WSIs, up to ~10GB per file), memory and storage constraints are anticipated for ATOM NPU–based services. Fortunately, ViT-MIL and CNN-MIL models require significantly smaller memory footprints, making deployment feasible.

Patch-level classification experiments (see Fig. 4) demonstrated visually distinct differences between metastatic and normal tissue patches, even for non-experts. However, imbalance in the number of cancer vs. normal patches introduces class imbalance issues.

<img width="423" height="253" alt="image" src="https://github.com/user-attachments/assets/17943aac-a451-459d-af56-bfbad25cd12f" />

Semantic segmentation methods, though requiring pixel-level annotations, avoid class imbalance and have been reported to achieve relatively high accuracy (AUC ~95%).

## Key Achievements

1) Developed a ViT-backbone MIL model robust to WSI-level weak labeling, avoiding OOM errors on low-memory GPUs.
2) Achieved 2–3% higher AUC compared to CNN-MIL baselines (90–95%).
