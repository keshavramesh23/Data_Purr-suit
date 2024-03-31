# IoMT
Developing Classification Models for detecting Internet of Medical Things (IoMT) attacks

# Preprocessing
## Stage 1: Retrieving and aggregating the Dataset

[The dataset](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html) contains the following attacks on IoMT devices, split into train and test files:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/f887468e-f1e4-4914-8fd2-7756e22060cd)

Each of the datasets (with information about the category of attack and attack performed) contains the following features:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/82a487db-71d7-4915-a2f9-94b1c70553ba)

Source: https://www.preprints.org/manuscript/202402.0898/v1

We aim to build the best classification model to classify attacks by category, not type. We decided to drop the Spoofing class because it is too imbalanced relative to the other categories. We will use various imbalanced data techniques to improve the classifier's accuracy and F1 score for the remaining five classes.

We will make our own train and test sets because we will do stratified sampling.
  1. First, we manually move all the attacks to a folder based on their category (DDoS, Recon, DoS, MQTT, Benign).
  2. Each category sometimes has various files for each attack. For example, in DDoS, there are 10 CSV files with ICMP attacks (including train and test files. We merge each of these files and rename the file to the attack name
  3. Next, we merge all the attacks into a CSV file called by their category name. While doing so, we create a new column called "Attack" and attach the file name (signifying which attack) to stratify the sample based on the attacks in the future.
  4. We now have 5 CSV files for each category. We merge these five to have a large dataset. While aggregating this dataset, we will add a new column called "Category," our target variable for our classification task.

## Stage 2: Preprocessing the combined dataset

1. Data Cleaning: Removing NaN values (we have enough entries, so we can afford to drop) and check for incorrect/corrupt entries
2. Data Transformation: Normalization and Scaling
3. Feature Encoding: For variables "Attack" and "Category," we label encode them since it's a multi-class classification problem (our target variable is Category, but we'll convert Attack in case we need it for later)
4. Identifying outliers and dropping near zero variance data?
5. Handling Imbalanced Data? (store this in a new data frame and experiment with it after establishing baselines)
6. Reduce dimensionality using PCA to observe results
7. Feature Selection (need to experiment with this afterward)

## Stage 3: Model Building

1. Which models? Autogluon
2. Cross Validation
3. Stratified Sampling
4. Hyperparameter Tuning
5. Experimenting with 1:2 and 1:3 samples
