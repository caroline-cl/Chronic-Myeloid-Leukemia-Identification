# Chronic-Myeloid-Leukemia-Identification

Chronic myeloid leukemia (CML), also known as chronic myelogenous leukemia, is a type of cancer that starts in certain blood-forming cells of the bone marrow, which differentiates itself from other cancers that start in organs like the lungs, breast, or colon and then spread to the bone marrow. It is essential to be able to differentiate CML cells from normal cells to help facilitate the process of identifying the cancer cell. In this project, I used the expression values of certain genes for given samples and their phenotype labels to build some generalizable machine learning models that classify each sample into the binary class of cell type (normal vs. CML).

## Workflow
My workflow is divided into three main sections: 1. Data preprocessing/analysis (train/test split, one-hot encoding, feature selection) 2. Methodology (experiment on different machine learning model, hyperparameter tuning) 3. Evaluation (result, model performance comparison, discussion/conclusion).


## Dataset
The dataset concerns an important problem of chronic myeloid leukemia identification. We are interested in differentiating the chronic myeloid leukemia from the normal control samples based on single-cell data analysis. The dataset contains 2151 cells and the expression value of 27118 genes. Phenotype of interest is the disease type of each cell. The population contains 187 normal cells and 1964 cells associated with chronic myeloid leukemia.

This dataset is a single-cell dataset, meaning it contains expressions measured within a single cell. It is more sparse than multi-cell dataset and has a reasonable number of zeros as entries. In this dataset, there are 2151 cells (samples) and 27118 genes expression value. Among the 2151 cells, 1964 of which are associated with chronic myeloid leukemia, while only 183 are normal cells. This indicates the data is extremely unbalanced. It is important to address this issue because, suppose we simply implement a model that outputs disease type as chronic myeloid leukemia for whatever inputs given, we would still have a 91.3% accuracy. There are certain aspects that I will focus on when addressing this imbalanced data issue: 1. Balance the data. 2. Cost-sensitive machine learning algorithm. 3. Suitable performance metric. The first thing to think of when encountering an imbalanced data, of course, is try to balance the data, such as doing SMOTE oversampling/random under-sampling. However, random oversampling may increase the likelihood of occurring overfitting, since it makes exact copies of the minority class examples. Under-sampling is also not favorable since we are already dealing with a small size of dataset (only 2151 samples). As a result, I decided to implement specialized modeling algorithms that are helpful when dealing with this type of unbalanced data, such as cost-sensitive logistic regression and cost-sensitive decision tree. Since using accuracy as the performance metrics may be misleading, we have to look at other metrics such as the ROC AUC.

## Feature Selection
There are over 20k genes and possible thresholds to consider every combination exhaustively, so it is important to perform feature selection to lower down the computational intensiveness. I used two different methods of feature selection. When we look at the correlations between features of the dataset, we can see that sometimes two features can be correlated. If a feature is highly correlated with another feature, it means they are more linearly dependent and have the same effect on the dependent variable. Thus, the two features bring redundant information and one of the features can be dropped. This naturally leads to the first method of using hypothesis testing, specifically the Wilcoxon rank-sum Test. I chose the top most differentially expressed genes which satisfies the requirement of having an adjusted p-value under the choice of threshold alpha = 0.03, or in another words, those that have rejected the null hypothesis (two features have the same continuous distribution).

The second one is to use correlation of features with the class labels as a metric for selection. This way we can remove the more irrelevant features to the target, which may help improve our prediction. Specifically, we select the top 100 features having the largest magnitude correlation values with the class label.

