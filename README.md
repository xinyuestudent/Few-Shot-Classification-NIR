## Engineering an Attention-Guided Adaptive Metric Network for Few-Shot Classification in Agricultural Spectroscopy

### The implementation code will be released under an open-source license contingent upon formal acceptance of the accompanying manuscript for publication.

| Author Name        |                    Affiliation                     |
| :----------------- | :------------------------------------------------: |
| **Xinyue Wang**    |         **Southwest Jiaotong University**          |
| **Yang Huang**     |   **China Tobacco Sichuan Industrial Co., Ltd.**   |
| **Xiangdong Chen** |         **Southwest Jiaotong University**          |
| **Ronggao Gong**   |         **Sichuan Agriculture University**         |
| **Tao Wang**       | **Mianyang Hospital of Chengdu University of TCM** |
| **Xiaomei Long**   |         **Southwest Jiaotong University**          |
| **Tailin She**     |   **China Tobacco Sichuan Industrial Co., Ltd.**   |
| **Yasen li**       |   **China Tobacco Sichuan Industrial Co., Ltd.**   |

### Motivation

Near-infrared (NIR) spectroscopy is a widely employed technique for rapid and non-destructive analysis of the chemical and physical properties of materials. However, NIR data frequently exhibit substantial variability owing to instrumental noise, environmental conditions, and inherent differences among the samples under analysis. Furthermore, distinct application domains (e.g., pharmaceuticals, food quality control, and agriculture) may exhibit unique data distributions, complicating the development of models that generalize effectively across diverse scenarios, and necessitating a flexible and adaptable learning framework.
	
A prevalent approach to addressing such variability involves collecting large annotated datasets for each domain; however, this practice is often impractical due to the considerable cost and time required for data acquisition and labelling. Furthermore, minor differences in spectra within the same class can result in challenges in distinguishing between classes, particularly when the available training data is constrained. These factors underscore the necessity for a robust and adaptable few-shot learning framework capable of effectively addressing minor differences in NIR data and adapting to varying data distributions across diverse domains.

### Dataset

| Dataset name          | No. of Samples | No. of Classes | Type of Class |
| --------------------- | -------------- | -------------- | ------------- |
| Mango Nir Dataset     | 10243          | 10             | Cultivar      |
| Pines Nir Dataset     | 752            | 2              | Cultivar      |
| Blueberry Nir Dataset | 1350           | 4              | Acidity       |
| Tree Nir Dataset      | 1271           | 4              | Organ series  |
| Cigar Nir Dataset     | 384            | 6              | Origin        |

![image-20250709162307725](image-20250709162307725.png)

### The Architecture of Proposed Method

![image-20250709162420026](image-20250709162420026.png)

### Implement

We train the models in both n-way n-shot settings with standard normalization and augmentation techniques as in previous works \cite{}. In addition, our attention blocks are shared between adaptation steps and only a single head (H=1) is used, with the size of this layer set to 640. During training, the Adam Optimizer is trained with a learning rate of 0.003 without decay or scheduling. The 1-shot model is trained for 200 epochs, with 200 tasks sampled in each epoch, resulting in 40,000 tasks. On the other hand, the 5-shot model is trained for 200 epochs, with a total of 40,000 tasks. During the evaluation, we sample 15 queries per class in each episode and report the average accuracy with its confidence interval over 2,000 randomly sampled test episodes. In this study, in order to ensure the consistency of the analysis, the SNV + SG + 1st derivative preprocessing was used for the qualitative analysis.

### Experiments and Results

**Table: Performance of various few shot models on multiple datasets.**

| Model          | Mango Nir (1-shot) | Mango Nir (5-shot) | Pine Nir (1-shot) | Pine Nir (5-shot) | Tree Nir (1-shot) | Tree Nir (5-shot) |
| :------------- | :----------------: | :----------------: | :---------------: | :---------------: | :---------------: | :---------------: |
| LSTM-FCN       |        0.57        |        0.65        |       0.71        |       0.81        |       0.75        |       0.85        |
| MAML           |       0.598        |       0.694        |       0.73        |       0.82        |       0.83        |       0.93        |
| SCNN           |        0.78        |        0.86        |       0.79        |       0.91        |       0.91        |         1         |
| MsmcNet        |        0.72        |       0.831        |       0.80        |       0.91        |       0.90        |         1         |
| AMCRN          |        0.82        |        0.93        |       0.81        |       0.92        |       0.89        |         1         |
| EmoDSN         |       0.678        |       0.787        |       0.82        |       0.93        |       0.88        |         1         |
| **APMN (Our)** |      **0.83**      |      **0.96**      |     **0.83**      |     **0.965**     |     **0.91**      |       **1**       |

**Table: Performance of various few shot models on multiple datasets.**

| Model           | Blueberry Nir (1-shot) | Blueberry Nir (5-shot) | Cigar Nir (1-shot) | Cigar Nir (5-shot) |
| :-------------- | :--------------------: | :--------------------: | :----------------: | :----------------: |
| LSTM-FCN (2017) |          0.71          |          0.81          |        0.35        |        0.52        |
| MAML (2017)     |          0.78          |          0.86          |        0.38        |        0.55        |
| SCNN (2021)     |          0.80          |          0.91          |        0.44        |        0.60        |
| MsmcNet (2022)  |          0.81          |          0.92          |        0.45        |        0.62        |
| AMCRN (2022)    |          0.84          |          0.94          |        0.48        |        0.64        |
| EmoDSN (2023)   |          0.81          |          0.93          |        0.51        |        0.65        |
| **APMN (Our)**  |        **0.85**        |        **0.95**        |      **0.55**      |      **0.76**      |
