# PROI-for-Misinformation-Detection
Repository of Partitioned Representation with Oriented Interaction for Multimodal Misinformation Detection

This repository contains the official implementation of the paper: **"Mitigating Shortcut Learning in Multimodal Misinformation Detection via LLM-Guided Orthogonal Disentanglement"**. 

In this work, we propose **PROI** (Partitioned Representation with Oriented Interaction), a novel framework that integrates LLM-guided knowledge distillation with explicit orthogonal decomposition to rigorously separate multimodal features into event-related facts and non-event components. Furthermore, we introduce an Out-of-Distribution (OOD) validation-guided training strategy to maximize model robustness against temporal evolution and domain shifts.

## 📁 Repository Structure

The repository is organized as follows to ensure reproducibility and clarity:

```text
PROI-for-Misinformation-Detection/
├── code/
│   ├── inference/
│   │   └── Inference.ipynb                 # Pre-trained model loading and evaluation script
│   ├── pre_process/
│   │   ├── preprocess_image.ipynb          # Image standardization and padding (224x224)
│   │   ├── preprocess_twitter.ipynb        # Text cleaning and merging for Twitter dataset
│   │   └── preprocess_weibo(weibo21).txt   # Notes on Weibo/Weibo21 preprocessing pipeline
│   └── train/
│       ├── twitter_train.ipynb             # Training script for Twitter15
│       └── weibo_train.ipynb               # Training script for Weibo
├── data/
│   ├── twitter/                            # Twitter15 Benchmark
│   │   ├── train/
│   │   │   └── tw15_train_news_description.xlsx
│   │   ├── test/
│   │   │   └── tw15_test_news_description.xlsx
│   │   └── OOD test/                       # Cross-platform evaluation
│   │       └── translated_weibo.xlsx       
│   └── weibo/                              # Weibo Benchmark
│       ├── train/
│       │   └── wb_train_news_description.xlsx
│       ├── valid/                          # OOD Validation (Temporal Shift)
│       │   └── weibo21_valid_use.xlsx
│       ├── test/
│       │   └── wb_test_news_description.xlsx
│       └── OOD test/                       # Cross-temporal & Cross-platform evaluation
│           ├── translated_twitter.xlsx
│           └── weibo21_test_use.xlsx
├── environment.yml                         # Conda environment configuration
├── LICENSE
└── README.md


## ⚠️ Data Availability Statement

Due to strict privacy and copyright restrictions associated with social media platforms, we do not have the authorization to publicly distribute the raw text and image files for the **Weibo**, **Twitter**, and **Weibo21** datasets. 

To ensure full reproducibility while respecting data usage policies, we have provided `.xlsx` files in the `data/` directory. These files contain the exact **Sample IDs** along with the necessary meta-labels used in our experiments.

**How to reconstruct the datasets:**
1. Please request and obtain the raw datasets directly from the original authors (refer to the dataset citations below).
2. Use our provided `.xlsx` files to filter the raw data by `id` and reconstruct the exact training, validation, and testing splits evaluated in this study.

**Dataset Citations:**
- **Weibo Dataset:** Jin, Z., Cao, J., Guo, H., Zhang, Y., & Luo, J. (2017). Multimodal fusion with recurrent neural networks for rumor detection on microblogs. In *Proceedings of the ACM Multimedia Conference* (pp. 795–816).
- **Twitter Dataset:** Boididou, C., Papadopoulos, S., Zampoglou, M., Apostolidis, L., Papadopoulou, O., & Kompatsiaris, Y. (2018). Detection and visualization of misleading content on Twitter. *International Journal of Multimedia Information Retrieval*, 7(1), 71–86.
- **Weibo21 Dataset:** Nan, Q., Cao, J., Zhu, Y., Wang, Y., & Li, J. (2021). MDFEND: Multi-domain fake news detection. In *Proceedings of the ACM International Conference on Information and Knowledge Management* (pp. 3343–3347).
