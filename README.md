# IoMT
Developing Classification Models for detecting Internet of Medical Things (IoMT) attacks

## Preprocessing

[The dataset ](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html) contains the following attacks on IoMT devices, split into train and test files:

![image](https://github.com/keshavramesh23/IoMT/assets/106554196/a78f40fb-fe01-4f93-ba25-c3a35951e1f8)

[Source:](https://www.preprints.org/manuscript/202402.0898/v1)

Our goal is to build the best classification model to classify attacks by category, not type. We decided to drop the Spoofing class because it is too imbalanced relative to the rest of the categories. We will use various imbalanced data techniques to attempt to improve the classifier's accuracy and F1 score for the remaining five classes.

2. We will make our own train and test sets because we're going to do stratified sampling
  1. First, we manually move all the attacks to a folder based on their category (DDoS, Recon, DoS, MQTT, Codebase).
  2. Each category sometimes has various files for each attack. For example, in DDoS, there are 10 CSV files with ICMP attacks (including train and test files. We merge each of these files and rename the file to the attack name
  3. Next, we merge all the attacks into a CSV file called by their category name. While doing so, we create a new column called "Attack" and attach the file name (signifying which attack) to stratify the sample based on the attacks in the future.
  4. We now have 5 CSV files for each category. We merge these five to have a large dataset. While aggregating this dataset, we will add a new column called "Category," our target variable for our classification task. 


  

