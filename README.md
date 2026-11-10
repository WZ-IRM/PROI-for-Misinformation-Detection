# PROI-for-Misinformation-Detection
Repository of Partitioned Representation with Oriented Interaction for Multimodal Misinformation Detection

This repository contains the official implementation of the paper: **"Mitigating Shortcut Learning in Multimodal Misinformation Detection via LLM-Guided Orthogonal Disentanglement"**. 

In this work, we propose **PROI** (Partitioned Representation with Oriented Interaction), a novel framework that integrates LLM-guided knowledge distillation with explicit orthogonal decomposition to rigorously separate multimodal features into event-related facts and non-event components. Furthermore, we introduce an Out-of-Distribution (OOD) validation-guided training strategy to maximize model robustness against temporal evolution and domain shifts.

## Important Note: Why Does Twitter Have No Validation Set?

The Twitter benchmark used in this repository provides only official training and test splits, with no designated validation set. In practice, prior studies frequently rely on test-set feedback for checkpoint selection and hyperparameter tuning. Even when studies claim to employ a validation set, its independence is often questionable or impossible to verify: some studies do not release their source code, while publicly available implementations we examined include cases where the official test set is directly used as validation data or for checkpoint selection. Others simply fail to explain how their validation sets were constructed. Consequently, using test-set feedback for model selection has become a common practice in this research area. Under this widely adopted benchmark protocol, our Twitter results on the standard i.i.d. test set are consistent with existing evaluation practices and can serve as a meaningful comparison with studies following the same protocol. However, we do not consider such i.i.d. performance sufficient evidence of genuine model capability or generalization. As demonstrated in Table 7 of our paper, the conventionally trained Twitter model achieves only 0.515 ACC when transferred to the translated Weibo dataset, indicating a near-complete loss of discriminative capability.

We attempted to construct an independent, temporally shifted OOD validation set for Twitter. However, this proved infeasible due to the lack of a suitable later dataset from the same platform (at least two years after the original benchmark) that preserves its characteristic format of short, image-text news posts. Furthermore, the original Twitter dataset suffers from substantial redundancy, with numerous samples sharing nearly identical semantics and describing the same events. Randomly splitting the data would inevitably introduce event overlap between training and validation sets (see the final section of this README, **"The Pitfalls of Existing Re-splitting Strategies"**, for a detailed discussion of why both random splitting and clustering-based re-splitting fail to guarantee strict event isolation). We therefore retain the official train/test protocol rather than introducing an unreliable validation split.

**Our primary research focus is generalization, not leaderboard optimization.** To investigate whether multimodal misinformation detectors can learn transferable semantic representations and retain their discriminative capability under temporal and cross-platform distribution shifts, we establish an independent OOD validation and testing protocol using Weibo21, alongside cross-platform evaluation with translated datasets. Our findings demonstrate that high performance on conventional i.i.d. benchmarks alone cannot establish real-world robustness, while OOD validation-guided model selection provides a more reliable direction for developing generalizable misinformation detectors.

## Citation

> **If you find this work useful in your research, please consider citing our paper:**
>
> Zhi Wang and Shiyan Ou. "Mitigating shortcut learning in multimodal misinformation detection via LLM-guided orthogonal disentanglement." *Information Processing & Management*, Volume 64, Issue 1, Part B, Article 105043, 2027.
>
> https://doi.org/10.1016/j.ipm.2026.105043


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
```


## ⚠️ Data Availability Statement

Due to strict privacy and copyright restrictions associated with social media platforms, we do not have the authorization to publicly distribute the raw text and image files for the **Weibo**, **Twitter**, and **Weibo21** datasets. 

**How to reconstruct the datasets:**
1. Please request and obtain the raw datasets directly from the original authors (refer to the dataset citations below).
2. Use our provided `.xlsx` files to filter the raw data by `id` and reconstruct the exact training, validation, and testing splits evaluated in this study.

**Alternatively, for instant reproduction:**
Although we cannot publicly release the raw datasets, we have encapsulated the preprocessed multimodal features into `.pkl` format for your convenience.
- **Data & Pre-trained Weights:** You can download the processed `.pkl` data from our Hugging Face data repository: (https://huggingface.co/datasets/WZ-IRM/PROI-for-Misinformation-Detection), and the pre-trained model weights from our model repository: (https://huggingface.co/WZ-IRM/PROI-Model-for-Misinformation-Detection/tree/main). Using these resources, you can seamlessly reproduce our training results or run the inference scripts right out of the box.
- **OOD Sets & LLM Descriptions:** If you are solely interested in the OOD validation/test sets or the objective news descriptions generated by Large Language Models (which do not contain restricted raw data), you can find them directly in the `.xlsx` files within the `data/` directory of this GitHub repository. All tables include an `id` column, allowing you to easily merge them with the raw datasets once you obtain them.

> [!IMPORTANT]
> **Crucial Restriction on Weibo21:** In strict compliance with the licensing terms specified by the authors of the Weibo21 dataset (which strictly prohibit the redistribution of the dataset or any of its derived data), our shared `.pkl` files **DO NOT** contain preprocessed features for Weibo21. We only provide the corresponding data IDs for Weibo21 in the `.xlsx` files. To reproduce the results for Weibo21, readers must first obtain the raw dataset from the original authors and reconstruct the splits using our provided IDs.

**A Note on LLM-Generated Objective Descriptions:**
In our `.xlsx` files, the objective descriptions are stored in four columns: `news_g1` (GPT-4.1), `news_g2` (GPT-4.1-MINI), `news_q` (Qwen-3-VL-Max), and `news_m` (Gemini-Flash-Lite). 
- Both the `train` and `test` sets for Weibo and Twitter contain all four columns. *Note: The descriptions in the test sets are strictly NOT used during training; they are provided solely for visualizing the disentanglement effects (please refer to our paper for visual results).*
- To optimize computational costs, the OOD datasets only contain the `news_g1` column, which is completely sufficient for visualization purposes.

**Dataset Citations:**
- **Weibo Dataset:** Jin, Z., Cao, J., Guo, H., Zhang, Y., & Luo, J. (2017). Multimodal fusion with recurrent neural networks for rumor detection on microblogs. In *Proceedings of the ACM Multimedia Conference* (pp. 795–816).
- **Twitter Dataset:** Boididou, C., Papadopoulos, S., Zampoglou, M., Apostolidis, L., Papadopoulou, O., & Kompatsiaris, Y. (2018). Detection and visualization of misleading content on Twitter. *International Journal of Multimedia Information Retrieval*, 7(1), 71–86.
- **Weibo21 Dataset:** Nan, Q., Cao, J., Zhu, Y., Wang, Y., & Li, J. (2021). MDFEND: Multi-domain fake news detection. In *Proceedings of the ACM International Conference on Information and Knowledge Management* (pp. 3343–3347).

## Rethinking the Training Paradigm: Shortcut Learning & The Validation Dilemma

This research focuses heavily on mitigating **"shortcut learning"** in multimodal misinformation detection — a critical bottleneck, especially for lightweight (non-LLM) vertical models. 

A prevalent but flawed "leaderboard-chasing" paradigm dominates current research: because standard benchmarks like Weibo and Twitter lack official validation sets, models are almost universally optimized by tuning hyperparameters directly on the test set. While this yields outstanding benchmark performance, it masks alarming real-world vulnerability. Motivated by this, our study introduces a strictly event-isolated **Out-of-Distribution (OOD) validation set** for the Weibo dataset and designs cross-temporal/cross-platform tasks. Our experimental results confirmed our hypothesis: variants trained via the traditional paradigm suffered severe performance degradation on OOD tasks, and the Twitter variant lost its detection capability entirely in cross-platform scenarios.

### Why Not Build an OOD Validation Set for Twitter15?
We extensively attempted to construct a similar temporal OOD validation set for the Twitter15 dataset but faced insurmountable data limitations. We could not identify a dataset that shares the same platform, is temporally asynchronous (at least 2 years later than Twitter15), and matches the stylistic profile (image-text, short news tweets). 

More critically, we observed that **the Twitter15 dataset (both train and test splits) suffers from extreme data redundancy and duplication**. A massive number of samples share nearly identical semantics describing the exact same events. This inherent flaw causes highly unstable convergence during training. While we cannot overcome these inherent dataset defects, our existing experiments sufficiently prove the absolute necessity of using an event-isolated validation set. We hope the community will develop higher-quality benchmarks that reflect the current landscape of social media platforms.

### The Pitfalls of Existing Re-splitting Strategies
Officially, Weibo and Twitter only provide train/test splits. We note that some prior studies have attempted to artificially partition these datasets to create validation sets, typically through two methods. However, we argue that both approaches introduce severe methodological flaws:

1. **Random Splitting from the Training Set:** This naive approach entirely ignores the fact that multiple posts describe the same event. Random splitting means that samples from the exact same event will appear in both the training and validation sets. This results in severe data leakage, allowing the model to effortlessly "guess" the labels based on memorized event entities rather than learning generalized detection features, thereby rendering the validation set useless.
2. **Clustering-based Resplitting:** This approach mixes the original train and test sets, clusters them by event, and resplits them to ensure event isolation. While it conceptually recognizes the leakage problem, it fatally overestimates the accuracy of clustering algorithms. Imperfect clustering inevitably leads to misclassified samples and subsequent event leakage.

In Conclusion, eradicating the "leaderboard-chasing" phenomenon on these traditional datasets is exceedingly difficult due to the challenge of establishing strict event isolation without breaking benchmark integrity. Moving forward, the community urgently needs new, strictly event-isolated benchmarks to replace these traditional datasets and accurately evaluate real-world robustness.
