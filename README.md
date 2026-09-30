# Enzyme Function Classification

A machine learning project for predicting the main enzyme class (EC 1–6) from protein amino acid sequences.

The project uses sequence-based feature engineering, multiple machine learning classifiers, and ensemble learning methods to classify proteins into their main Enzyme Commission (EC) classes.

## Dataset

The project uses the `DanielHesslow/SwissProt-EC` dataset from Hugging Face.

After preprocessing, 169,682 usable protein sequences were available. Due to computational limitations, a random sample of 10,000 sequences was used for the experiment.

The data was divided using a stratified split:

- 70% training: 7,000 sequences
- 15% validation: 1,500 sequences
- 15% testing: 1,500 sequences

The preprocessing steps included removing missing and duplicate sequences, handling non-standard amino acid characters, and extracting the first digit of the EC number as the target class.

For example:

```text
EC:2.7.1.1 -> Class 2
```

This resulted in six target classes, EC 1 through EC 6.

## Feature Engineering

Several sequence representations were used:

- **Amino Acid Composition (AAC):** 20 features
- **Dipeptide Composition (DPC):** 400 features
- **Tripeptide features:** 100 features selected using `SelectKBest` with mutual information
- **TruncatedSVD:** 128 features
- **Word2Vec:** 64-dimensional sequence embeddings

The final combined representation contained:

```text
20 + 400 + 100 + 128 + 64 = 712 features
```

## Class Imbalance

The dataset contains an unequal number of samples across the six enzyme classes.

SMOTE was included inside the model training pipelines so that oversampling was applied only to training data during cross-validation and not to the validation or test sets.

## Models

The following models were trained and evaluated:

- Random Forest
- LightGBM
- Support Vector Machine (SVM)
- XGBoost
- Multilayer Perceptron (MLP)

The main models were tuned using 5-fold cross-validation with Macro F1 as the primary evaluation metric.

Due to its computational cost, SVM hyperparameter tuning was performed using a random subset of 3,000 training samples.

## Ensemble Methods

Three ensemble methods were evaluated:

- Hard Voting
- Soft Voting
- Stacking

The models were compared using validation Macro F1. The model with the highest validation Macro F1 was selected for final evaluation on the test set.

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

Stacking achieved the highest validation Macro F1 and was selected as the final model.

### Test Results

The final Stacking model achieved:

| Metric | Score |
| --- | ---: |
| Macro F1 | 0.6104 |
| Accuracy | 0.6353 |
| MCC | 0.5342 |
| AUPRC | 0.6996 |
| AUC | 0.8778 |

Out of 1,500 test sequences, 953 were classified correctly and 547 were misclassified.

Class 6 achieved the highest class-level F1 score at approximately 0.74, while Class 5 had the lowest at approximately 0.47.

## Running the Notebook

The project was developed and executed using **Google Colab**.

Open the Jupyter Notebook in Google Colab and run the cells from top to bottom.

The required Python packages are installed from within the notebook:

```bash
pip install datasets lightgbm imbalanced-learn shap scikit-learn matplotlib seaborn gensim xgboost
```

The notebook follows the workflow:

```text
Data Loading
    ↓
Preprocessing
    ↓
Feature Engineering
    ↓
Model Training and Tuning
    ↓
Ensemble Evaluation
    ↓
Final Test Evaluation
    ↓
Error Analysis
```

## Limitations

Only 10,000 sequences were used from the larger processed dataset because of computational limitations.

The models rely on sequence-derived features and do not incorporate additional biological information such as protein 3D structure or active-site information.

The number of selected tripeptide features was fixed at 100, and SVM hyperparameter tuning was performed on a smaller subset of the training data.

Future work could evaluate larger training samples, different feature-selection settings, additional protein representations, and alternative classification approaches.

## References

- DanielHesslow/SwissProt-EC Dataset, Hugging Face
- Scikit-learn
- LightGBM
- XGBoost
- imbalanced-learn
- Gensim
