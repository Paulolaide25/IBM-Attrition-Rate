# IBM Attrition Rate: Model Comparison

Machine learning comparison for predicting employee attrition on the IBM HR Analytics dataset (1,470 employees, 35 fields). Three classifiers are trained and compared, then the best-performing family (Random Forest) is tuned with a grid search.

> Companion repo: [IBM-Employee-Attrition](https://github.com/Paulolaide25/IBM-Employee-Attrition) covers the exploratory analysis and what drives attrition.

![Model comparison](images/model-comparison.png)

## Approach
1. Cleaned the data and dropped constant or ID columns (`EmployeeCount`, `EmployeeNumber`, `Over18`, `StandardHours`).
2. Encoded the target (Yes/No to 1/0) and one-hot encoded the categorical fields.
3. Split 80/20 into train and test sets (294 test employees, 49 of whom left).
4. Trained Logistic Regression, Random Forest and Decision Tree models, then compared them.
5. Tuned the Random Forest with `GridSearchCV`.

## Results on the test set
| Model | Accuracy | Precision (leavers) | Recall (leavers) | F1 (leavers) |
|---|---|---|---|---|
| Logistic Regression | 0.84 | 0.53 | 0.20 | 0.29 |
| Random Forest | 0.85 | 0.78 | 0.14 | 0.24 |
| Decision Tree | 0.79 | 0.37 | 0.37 | 0.37 |

**What this shows:** Random Forest has the highest accuracy and is the most precise when it does flag a leaver, but it misses most of them. The Decision Tree catches the most leavers (recall 0.37) at the cost of overall accuracy. Only about 16% of employees left, so accuracy alone is misleading, and the right model depends on whether HR wants fewer false alarms or fewer missed leavers.

The grid search selected `n_estimators=200`, `max_depth=20`, `max_features='sqrt'`, `min_samples_split=2` and `min_samples_leaf=1`.

## Next steps
Address class imbalance (SMOTE or class weights), tune the decision threshold, and report ROC-AUC.

## Tools
Python, pandas, NumPy, scikit-learn, matplotlib, seaborn.

## Repository contents
| File | Description |
|---|---|
| `IBM_ATTRITION_DATA_3_MLA.ipynb` | Notebook with the full workflow |
| `IBM Employee Attrition.xlsx` | Source dataset |

## How to run
```bash
pip install pandas numpy scikit-learn matplotlib seaborn openpyxl jupyter
jupyter notebook IBM_ATTRITION_DATA_3_MLA.ipynb
```
The notebook was written in Google Colab and reads the data from Google Drive. To run it locally, change the `read_excel` path to `IBM Employee Attrition.xlsx` and remove the Drive mount cell.
