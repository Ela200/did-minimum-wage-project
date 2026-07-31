# Baseline Model

**[Notebook](baseline_model.ipynb)**

## Baseline Model Results

### Model Selection
- **Baseline Model Type:** No Controls Doubly Robust DiD estimator (DummyRegressor for the outcome model, DummyClassifier for the treatment model)
- **Rationale:** This model uses no covariates. It reduces to the classic unconditional Difference in Differences estimator. It is the standard method that the more advanced learners in this project (Lasso, Ridge, Random Forest, and the Best/Stack ensembles) are meant to improve on. It needs no tuning, so it is simple to reproduce and gives a clean reference point for the rest of the project.

### Model Performance
- **Evaluation Metric:** ATT (Average Treatment Effect on the Treated), standard error of the ATT, and cross-fitted RMSE for the outcome model and the treatment model
- **Performance Score:**
  - 2004: ATT = -0.040, SE = 0.019, RMSE dy = 0.163, RMSE D = 0.198
  - 2005: ATT = -0.076, SE = 0.020, RMSE dy = 0.188, RMSE D = 0.201
  - 2006: ATT = -0.117, SE = 0.020, RMSE dy = 0.223, RMSE D = 0.211
  - 2007: ATT = -0.131, SE = 0.023, RMSE dy = 0.230, RMSE D = 0.250
- **Cross-Validation Score:** RMSE values above come from 5 fold cross-fitting, repeated for each year

### Evaluation Methodology
- **Data Split:** No train/validation/test split. We use 5 fold cross-fitting instead, which is standard for Double Machine Learning. Each fold is used once for prediction while the model is trained on the other folds, so every observation gets an out-of-sample prediction.
- **Evaluation Metrics:** ATT and its standard error measure the causal effect and its precision. RMSE for the outcome and treatment models measures how well each nuisance model predicts, which matters because DML relies on those predictions being accurate.

### Metric Practical Relevance
The ATT tells us the estimated change in log teen employment caused by the minimum wage increase, for counties that received it. A negative ATT means the policy is associated with a drop in teen employment relative to what would have happened without it. The estimate becomes more negative over time, from -0.040 in 2004 to -0.131 in 2007, which suggests the effect builds up gradually rather than hitting all at once. The RMSE values matter because this baseline uses no covariates. When later models add controls and get a lower RMSE, that tells us the covariates carry real predictive signal, and we can then check whether the ATT estimate also changes once we account for it.

## Next Steps
This baseline model serves as a reference point for evaluating more sophisticated models in the [Model Definition and Evaluation](../3_Model/README.md) phase.
