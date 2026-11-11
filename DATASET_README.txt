Dataset Title
Vibration-Based Pipeline Leak Detection Dataset Using Linear Discriminant Analysis (LDA)
Description

This repository contains the experimental vibration data, extracted feature dataset, trained model parameters, and analysis scripts used in the study:
“Vibration-Based Leak Detection in fluid Pipelines Using Linear Discriminant Analysis.”
The dataset supports the results presented in the manuscript and enables full reproducibility of the experimental and machine-learning procedures.

Experimental Overview
Vibration signals were collected from PVC pipeline spans of:
•	1.0 m
•	1.5 m
•	2.0 m
under two flow conditions:
•	45 L/min (slow flow)
•	60 L/min (fast flow)
Note: Fast flow was only taken for the first and last positions of each span. E.g for 1m span, 0m and 1m position.
Each span was tested under:
•	Healthy condition
•	Defective (leak) condition
Acceleration data were recorded using an MPU6050 accelerometer measuring three-axis signals (Ax, Ay, Az).
File Descriptions
1. Raw Data (raw_data/)
Each CSV file contains time-series acceleration data with the following columns:
•	Time (seconds)
•	Ax (g)
•	Ay (g)
•	Az (g)
Each file corresponds to a specific pipe span, condition, and flow rate.
2. Extracted Feature Dataset (features/)
extracted_features.csv contains the feature matrix used for training and evaluation.
Columns include:
•	Time-domain features: RMS, standard deviation, kurtosis, skewness, entropy
•	Frequency-domain features: spectral centroid, band power, spectral entropy
•	Label (0 = Healthy, 1 = Defective)
Each row corresponds to one segmented window of vibration data.
3. Model Parameters (model/)
•	lda_coefficients.csv – Weight vector (w) for the trained LDA model
•	lda_intercept.txt – Intercept term (b)
•	threshold.txt – Decision threshold (0.61)
The discriminant function is defined as:
S = wᵀx + b
The probability is computed as:
P = 1 / (1 + e^(−S))
Classification rule:
If P ≥ 0.61 → Defective
Else → Healthy
4. Code (code/)
•	feature_extraction.py – Script for computing statistical and spectral features
•	train_lda.py – Script for training the LDA model
•	predict.py – Script for generating predictions from new vibration data
All scripts were implemented in Python 3.10 using:
•	NumPy
•	Pandas
•	SciPy
•	scikit-learn
Reproducibility Instructions
1.	Load raw data from the raw_data/ folder.
2.	Run feature_extraction.py to compute features.
3.	Use train_lda.py to reproduce the trained model.
4.	Apply predict.py for classification.
The reported performance metrics (accuracy ≈ 81%, F1-score ≈ 72.5%) can be reproduced using stratified 5-fold cross-validation.
Contact
For questions regarding the dataset or analysis, please contact:
Ariyo Theophilus Oluwaseyi
ariyotheophilus@gmail.com
License
This dataset is provided for research and academic use only.


