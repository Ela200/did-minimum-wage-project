# Model Definition and Evaluation

**[Notebook](model_definition_evaluation.ipynb)**

## Model Selection

We move beyond the "No Controls" baseline and consider several ML methods as candidate nuisance learners for the doubly robust DML-DiD estimator: Basic (plain linear/logistic regression), Expansion (linear/logistic with region interactions), Lasso (CV), Ridge (CV), and Random Forest. We also build two ensembles on top: "Best" (lowest cross-fitted RMSE per nuisance function) and "Stack" (linear combination of all learners' out-of-fold predictions). We considered this range of methods, from simple linear models to regularized high-dimensional models to tree ensembles, because we did not know in advance which functional form best captures how baseline county characteristics relate to the outcome and treatment. Rather than picking one model upfront, we let cross-fitted performance decide.

## Model Performance

- **Evaluation Metric:** ATT (Average Treatment Effect on the Treated), standard error of the ATT, and cross-fitted RMSE for the outcome and treatment nuisance models
- **Performance Score (2007 example):**
  - No Controls: ATT = -0.131, RMSE dy = 0.230, RMSE D = 0.250
  - Basic: ATT = -0.067, RMSE dy = 0.222, RMSE D = 0.221
  - Ridge (CV): ATT = -0.064, RMSE dy = 0.222, RMSE D = 0.230
  - Random Forest: ATT = -0.067, RMSE dy = 0.223, RMSE D = 0.224
  - Best: ATT = -0.077, RMSE dy = 0.222, RMSE D = 0.220
  - Stack: ATT = -0.065, RMSE dy = 0.221, RMSE D = 0.217
  - (Lasso (CV) is the one exception: ATT = -0.062, RMSE dy = 0.222, but RMSE D = 0.274, worse than the baseline)
  - Full results for 2004-2007 are in the notebook.
- **Cross-Validation Score:** All RMSE and ATT values come from 5-fold cross-fitting per year, repeated for years 2004-2007.

## Evaluation Methodology

- **Data Split:** No train/validation/test split. As with the baseline, we use 5-fold cross-fitting, which is standard for Double Machine Learning, so every observation gets an out-of-sample nuisance prediction.
- **Evaluation Metrics:** ATT and SE measure the causal effect and its precision. RMSE for the outcome and treatment nuisance models measures how well covariates help predict each, which matters because DML relies on those predictions being accurate.
- **Pre-trends check:** We reran the same pipeline treating 2002 as a placebo post-period. All methods produced ATT estimates close to zero (-0.005 to 0.010) with standard errors around 0.013-0.022, none statistically distinguishable from zero. This supports the conditional parallel trends assumption underlying the DiD design.

## Metric Practical Relevance

Every covariate-adjusted method produces a smaller (less negative) ATT than "No Controls" in every year. In 2007, "No Controls" estimates a 13.1% drop in teen employment, while covariate-adjusted methods estimate roughly 6-8%, about half the baseline's magnitude. This tells us that ignoring baseline county characteristics overstates the negative employment effect, since counties that raised their minimum wage were not identical, on average, to counties that did not, even before treatment. RMSE improvements from adding covariates are modest but consistent for the outcome model. Lasso is the one clear underperformer, its treatment-model RMSE is worse than the baseline in every year, likely because the L1 penalty shrinks too many of the third-order polynomial interaction terms to zero and loses signal. We recommend "Stack" as the headline result, since it consistently achieves the lowest or near-lowest RMSE and its ATT estimates fall in a similar range as the other well-performing methods, meaning the covariate-adjusted conclusion does not hinge on picking one specific functional form.

## Heterogeneity Analysis

As an extension, we used a Causal Forest (`econml`'s `CausalForestDML`) to examine whether the treatment effect varies across counties rather than assuming one constant effect. The Causal Forest's average treatment effect estimates are broadly consistent with the DML results above (-0.032 in 2004 to -0.102 in 2007), though its confidence intervals are wide and include zero in every year, expected since partitioning the data to estimate heterogeneity is less sample-efficient than estimating a single average effect. The individual-level effect estimates (CATEs) show real spread: in 2007 they range from -1.458 to 0.213 with a standard deviation of 0.163. Feature importances point to baseline county population as the strongest driver of this heterogeneity (importance 0.40), followed by baseline average pay and baseline employment. The correlation between the estimated 2007 effect and baseline population is 0.546, and with baseline average pay is 0.353, both positive, suggesting the negative employment effect is concentrated in smaller, lower-population counties and eases somewhat in larger or higher-pay counties. Given the width of the CATE range and the ATE confidence intervals, these heterogeneity patterns should be read as suggestive rather than conclusive.

## Next Steps

This notebook, together with the baseline model, completes the modeling and evaluation phase of the project.
