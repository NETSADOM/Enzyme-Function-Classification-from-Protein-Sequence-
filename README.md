# Enzyme Function Classification

This project is about classifying protein sequences into the six main enzyme classes (EC 1–6) using machine learning.

I used the SwissProt-EC dataset from Hugging Face (`DanielHesslow/SwissProt-EC`). After cleaning the dataset, I used a sample of 10,000 sequences since running the full dataset was too computationally expensive.

The project was implemented and run on Google Colab.

## What I did

The protein sequences first had to be converted into numerical features that could be used by the models. I used:

- Amino Acid Composition (AAC)
- Dipeptide Composition (DPC)
- Tripeptide (3-mer) features
- TruncatedSVD
- Word2Vec embeddings

After combining these, each sequence had 712 features.

The data was split into 70% training, 15% validation, and 15% testing. I used stratified splitting to keep the class distribution similar between the three sets.

Since some enzyme classes had more samples than others, I used SMOTE during training to deal with the class imbalance.

## Models

I trained and compared:

- Random Forest
- LightGBM
- SVM
- MLP
- XGBoost

I also tried three ensemble methods:

- Hard Voting
- Soft Voting
- Stacking

The models were compared mainly using Macro F1. Stacking had the highest validation Macro F1, so I used it for the final test.

## Results

| Model | Macro F1 | Accuracy |
| --- | ---: | ---: |
| Random Forest | 0.5832 | 0.6087 |
| LightGBM | 0.6292 | 0.6547 |
| SVM | 0.4117 | 0.4400 |
| MLP | 0.5655 | 0.5880 |
| XGBoost | 0.6207 | 0.6467 |
| Hard Voting | 0.6257 | 0.6507 |
| Soft Voting | 0.6247 | 0.6440 |
| Stacking | **0.6313** | 0.6387 |

Final Stacking results on the test set:

- Macro F1: **0.6104**
- Accuracy: **63.53%**
- MCC: **0.5342**
- AUPRC: **0.6996**
- AUC: **0.8778**

Class 6 had the highest F1 score (0.74), while Class 5 had the lowest (0.47).

## Running the project

The whole project is in the Jupyter notebook and was run using Google Colab.
Install the required dependencies:

pip install datasets lightgbm imbalanced-learn shap scikit-learn matplotlib seaborn gensim xgboost

Then run the notebook cells from top to bottom.

Main libraries used:

```text
scikit-learn
lightgbm
xgboost
imbalanced-learn
gensim
datasets
matplotlib
seaborn
```

## Limitations

I only used 10,000 sequences because of the time and computational resources required to process the full dataset.

The project also only uses information extracted from the protein sequences. Other biological information, such as protein structure and active sites, was not included.

Using more of the available data and trying different feature representations could be explored in future work.
