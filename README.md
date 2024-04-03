# Data Purr-suit: The IoMT Cat-and-Mouse Game
Developing Classification Models for detecting Internet of Medical Things (IoMT) attacks

# Preprocessing

## Stage 1: Retrieving and aggregating the Dataset

The dataset [[1]](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html) contains the following attacks on IoMT devices, split into train and test files:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/f887468e-f1e4-4914-8fd2-7756e22060cd)

Each of the datasets (with information about the category of attack and attack performed) contains the following features:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/82a487db-71d7-4915-a2f9-94b1c70553ba)

Source: [[1]](https://www.preprints.org/manuscript/202402.0898/v1)

We aim to build the best classification model to classify attacks by category, not type of attack. 

We will make our train and test sets out of all the available data.
  1. First, we manually move all the attacks to a folder based on their category (DDoS, Recon, DoS, MQTT, Spoofing, Benign).
  2. Each category sometimes has various files for each attack. For example, in DDoS, there are 10 CSV files with ICMP attacks (including train and test files. We merge each of these files and rename the file to the attack name)
  3. Next, we merge all the attacks into a CSV file named after their category. While doing so, we create a new column called "Attack" and fill it with its specific attack type.
  4. We now have 6 CSV files for each category. We merge these six to have a large dataset. While aggregating this dataset, we will add a new column called "Category," our target variable for our classification task, which will be filled with its specific category.

A preview of the aggregated dataset is below: 

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/8f06a411-8ebf-4808-8117-d6f57bf0e549)

## Stage 2: Preprocessing the combined dataset

1. Data Cleaning: We checked for Null Values and found no null values. 
2. Data Transformation (Normalization and Scaling): Performed MinMaxScaling, a feature scaling technique that rescales features between [0, 1], by subtracting the minimum value and dividing by the range of the values.
3. Feature Encoding: For variables "Attack" and "Category," we label encode them since it's a multi-class classification problem (our target variable is Category, but we'll convert Attack in case we need it for later)

# Benchmarks

[[1]](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html) tested various classifiers on their train and test sets. The following highlights the results of their models:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/5083c4cf-8d97-4fd0-bff0-177262ae8f0a)

It is important to note that their train and test splits are different from ours, which will result in different metrics. As such, we will be establishing our benchmarks in the next stage.

# Model Building

For our testing, we ran the same models (Logistic Regression, AdaBoost, Deep Neural Network, Random Forest) to establish a benchmark for the six categories. 

Initially, we ran the model on every category except "Spoofing" since spoofing is a very imbalanced class, which would decrease our classifiers' accuracy. The classification report of the best-performing model (Random Forest) is given below:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/70b06ad4-66c9-4f4f-a9ed-8a1fabc0c32f)

From this, we can see that the Random Forest Model accurately predicts almost all categories of attacks and is incredibly good at distinguishing each kind of attack. Since there isn't much to improve on for this particular problem, we decided to include the category "Spoofing" to see the results of the classifiers.

A summary of the results of the classifiers for all six categories of attacks can be found below:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/f1402302-318d-4dfc-97b4-9167247c693b)

Once again, Random Forest is the best-performing model, with a weighted F1-score of 99.9012% and an Accuracy of 99.9020%. The classification report and the confusion matrix of this model are seen below:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/c9948651-3a3e-4508-93c4-bb35c00143cf)

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/187089ea-d3c3-4a01-8e57-8dc6e971b951)

The "Spoofing" class's per-class F1 score is relatively lower than the F1 scores of the other classes. This makes sense, considering the number of "Spoofing" instances is significantly lower than the other categories. 

# Next Steps:

1. The most consistent issue is the imbalanced data caused by the "Spoofing" class. Our efforts will improve the class's per-class accuracy and F1 score using class-imbalanced techniques such as Stratified Sampling, Oversampling, and Undersampling.
2. Dimensionality Reduction: The dataset currently consists of 45 features. Reducing the complexity of the model while maintaining similar accuracies would go a long way toward reducing model complexity and making it easier to integrate into real-world systems. We can do this by using
  a. Dimensionality Reduction techniques such as Principal Components Analysis (PCA)
  b. Feature Selection Techniques that help us select only the most important features to build our models. This can be done with built-in features from Ensemble Models (RF, AdaBoost) or by using other algorithms to evaluate the importance of features.

# References

[1] Dadkhah, S.; Carlos Pinto Neto, E.; Ferreira, R.; Chukwuka Molokwu, R.; Sadeghi, S.; Ghorbani, A. CICIoMT2024: Attack Vectors in Healthcare devices-A Multi-Protocol Dataset for Assessing IoMT Device Security. Preprints 2024, 2024020898. https://doi.org/10.20944/preprints202402.0898.v1
