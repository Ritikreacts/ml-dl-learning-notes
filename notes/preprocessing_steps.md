# Day 4 — Data Preprocessing Steps

**Goal:** Convert raw, messy tabular data into clean, numeric, model-ready data — without leaking test set information into training.

## Exact Steps

1. **Drop identifier columns**
   Remove columns like `SK_ID_CURR` — they're just row IDs, not predictive features.

2. **Separate features from target**
   `X` = all columns except the target
   `y` = the target column (e.g. `TARGET`)

3. **Separate columns by type**
   Split `X` into numerical columns and categorical columns — each needs different preprocessing.

4. **Define preprocessing recipe for numerical columns**
   - Fill missing values → median
   - Scale → StandardScaler

5. **Define preprocessing recipe for categorical columns**
   - Fill missing values → constant value (e.g. "missing")
   - Encode → OneHotEncoder

6. **Combine both recipes using ColumnTransformer**
   Routes numerical columns to the numeric recipe and categorical columns to the categorical recipe automatically.

7. **Split into train/test sets**
   This happens **before** any preprocessing is actually applied to the data — this is the critical anti-leakage rule.

8. **Fit on train only, transform both**
   - `fit_transform` on train → learns median, mean/std, category list from train data only
   - `transform` (no fit) on test → reuses what was learned from train, never relearns from test

## Why the order matters (leakage)

If you preprocess the *full* dataset before splitting, statistics like the median or average get calculated using test data too. That means your model indirectly "sees" the test set before evaluation — making performance look better than it really is. Splitting first and fitting only on train keeps the test set truly unseen.
