# Day 5 Notes — Regression & Linear Regression

Format per topic: what it means | where used | when to use | when not to use | common mistake

---

## 1. Regression (vs Classification)

**What it means:** Predicting a continuous numerical value (a number), not a category.
**Where used:** House-price prediction, sales prediction, delivery-time prediction, demand forecasting.
**When to use:** Target variable is a number (price, temperature, count).
**When not to use:** Target is a category (e.g. spam/not spam) — that's classification instead.
**Common mistake:** Confusing regression with classification when target looks numeric but is actually a category code (e.g. "1 = low, 2 = medium, 3 = high").

---

## 2. Best-fit line (Simple Linear Regression)

**What it means:** A straight line `y = m*x + b` that best represents the relationship between one feature (x) and the target (y).

- `m` (slope) = how much y changes per unit increase in x
- `b` (intercept) = predicted y when x = 0

**Where used:** Simple relationships — one feature predicting one target (e.g. hours studied → marks scored).
**When to use:** Relationship between feature and target looks roughly linear.
**When not to use:** Relationship is curved/non-linear — a straight line will underfit.
**Common mistake:** Assuming a straight line always fits — always plot the data first.

---

## 3. Multiple Linear Regression

**What it means:** Same idea as best-fit line, but with many features instead of one: `y = m1*x1 + m2*x2 + ... + b`
**Where used:** Real-world problems — house price depends on size, location, age, etc., not just one feature.
**When to use:** More than one feature likely influences the target.
**When not to use:** Features are heavily correlated with each other (multicollinearity) — coefficients become unstable/hard to interpret.

**Common mistake:** Not checking for multicollinearity before trusting individual coefficient values.

---

## 4. Residual / Error

**What it means:** `Actual value − Predicted value` for a single data point. Tells you how wrong the model was for that one point.
**Where used:** Building the cost function; diagnosing where a model is failing.
**When to use:** Always check residuals to sanity-check a regression model.
**When not to use:** N/A — always relevant.
**Common mistake:** Adding up raw residuals to judge model quality — positive and negative errors cancel out, hiding real problems. (This is why we square them — see Cost Function below.)

---

## 5. Cost Function (Mean Squared Error during training)

**What it means:** One single number summarizing how wrong the model's line is overall — average of squared residuals.
**Where used:** Training — the algorithm tries to minimize this number.
**When to use:** Needed any time a model has to "learn" — gives it something concrete to minimize.
**When not to use:** N/A — every trainable model needs some cost/loss function.
**Common mistake:** Forgetting that a *smaller* cost function value = a *better* model (not bigger).

---

## 6. Gradient Descent

**What it means:** A step-by-step algorithm to find the best `m` and `b`:
1. Start with a random guess for m, b
2. Measure error using the cost function
3. Adjust m, b a little at a time, always in the direction that reduces error
4. Repeat until error stops shrinking

**Where used:** Training Linear Regression (and most other ML/DL models).
**When to use:** Whenever you need to find parameters that minimize a cost function, especially with large datasets.
**When not to use:** For very simple/small Linear Regression problems, a direct formula (Normal Equation) may solve it in one shot without needing iterative steps — but gradient descent still works and scales better to large data.

**Common mistake:** Learning rate too large → overshoots the minimum and never converges. Too small → takes forever to converge.

---

## 7. MAE (Mean Absolute Error)

**What it means:** Average of the absolute value of residuals. Same unit as the target (e.g. lakhs).
**Where used:** Reporting model performance in a way that's easy to explain to non-technical stakeholders.
**When to use:** When you want errors treated equally regardless of size, or when outliers shouldn't be over-punished.
**When not to use:** When large errors are especially costly and should be penalized more (use MSE/RMSE instead).
**Common mistake:** Using MAE as the training loss when the goal actually needs big-error sensitivity.

---

## 8. MSE (Mean Squared Error)

**What it means:** Average of squared residuals. Penalizes big errors more heavily than small ones.
**Where used:** Most common loss function during training.
**When to use:** When large errors are especially bad and should be discouraged strongly.
**When not to use:** When reporting to non-technical audiences — the unit (e.g. lakhs²) isn't intuitive.

**Common mistake:** Reporting raw MSE to stakeholders instead of RMSE (which restores the original unit).

---

## 9. RMSE (Root Mean Squared Error)

**What it means:** Square root of MSE. Same unit as the target, but still penalizes big errors more than MAE does.
**Where used:** Reporting model performance when both interpretability and big-error sensitivity matter.
**When to use:** Default choice for reporting regression model error to humans.
**When not to use:** N/A — generally safe default alongside R².

**Common mistake:** Treating RMSE and MAE as interchangeable — RMSE will be equal or higher than MAE, and the gap tells you how much big outlier errors are affecting the model.

---

## 10. R² (R-squared)

**What it means:** How much better the model is than a "dumb" baseline that always predicts the average target value. Scale: 1 = perfect, 0 = same as guessing the average, negative = worse than guessing the average.

**One-liner to remember:** R² = how much better my model is than just guessing the average, on a scale of 0 to 1.
**Where used:** Reporting overall model quality as a single, easy-to-communicate score.
**When to use:** Comparing models, or explaining "how good" a model is to stakeholders.
**When not to use:** As the only metric — a high R² doesn't guarantee good predictions on new/unseen data (can still overfit).

**Common mistake:** Assuming R² close to 1 always means a great model, without checking overfitting (train R² vs test R²).

---

## 11. Loss vs Metric

**What it means:**
- **Loss** = the number the model actually optimizes during training (e.g. MSE via gradient descent)
- **Metric** = the number you report afterward to judge/communicate performance (e.g. RMSE, R²) — doesn't have to match the loss

**Where used:** Every ML training + evaluation workflow.

**When to use:** Always distinguish the two — pick loss based on what trains well, pick metric based on what's interpretable/business-relevant.

**When not to use:** N/A.

**Common mistake (🔴 classic interview trap):** Assuming loss and metric must be the same thing.

---

## 12. Regularisation (Ridge, Lasso)

**What it means:** Techniques that discourage a model from assigning very large weights to features, to prevent overfitting.

- **Ridge (L2):** shrinks all coefficients toward zero, keeps every feature in the model
- **Lasso (L1):** can shrink some coefficients to exactly zero → automatically performs feature selection
- **Elastic Net:** blend of both (know it exists — overview level only)

**Where used:** Any regression model at risk of overfitting, especially with many features.

**When to use:** Ridge — when you believe most/all features matter a little. Lasso — when you suspect many features are irrelevant and want the model to drop them automatically.

**When not to use:** Plain Linear Regression with very few, clearly relevant features and low overfitting risk — regularisation may be unnecessary overhead.

**Common mistake:** Not scaling features before applying Ridge/Lasso — regularisation penalizes coefficient size, so unscaled features (different ranges) get penalized unfairly.

---

## Quick Recap Table

| Concept | One-line memory hook |
|---|---|
| Regression | Predicting a number |
| Best-fit line | y = m*x + b |
| Residual | Actual − Predicted |
| Cost function | One number = how bad the line is |
| Gradient Descent | Nudge m,b until error stops shrinking |
| MAE | Avg absolute error, easy to explain |
| MSE | Avg squared error, punishes big misses |
| RMSE | √MSE, same unit as target |
| R² | How much better than guessing the average (0 to 1) |
| Loss vs Metric | Loss = trains the model, Metric = reports it |
| Ridge | Shrinks all coefficients, keeps all features |
| Lasso | Can zero-out coefficients, does feature selection |