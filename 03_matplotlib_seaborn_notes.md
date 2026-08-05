# Matplotlib & Seaborn — Day 1 Notes

### Figure and Axes
- **Means:** `Figure` = whole canvas; `Axes` = one plot inside it.
- **Used:** Any chart, especially when multiple subplots are needed.
- **Use when:** Use the `fig, ax = plt.subplots()` pattern for anything beyond a single throwaway plot.
- **Don't use when:** `plt.plot()` shortcut is fine only for a single quick, disposable chart.
- **Common mistake:** Using `plt.` calls directly when building multiple subplots, losing track of which axes is being drawn on.

### Histograms
- **Means:** Shows the distribution/shape of one numeric variable.
- **Used:** Checking skew, spread, and modality of a column.
- **Use when:** One continuous numeric variable.
- **Don't use when:** Categorical data — use a count/bar plot instead.
- **Common mistake:** Using a bar chart for continuous data instead of a histogram.

### Box plots
- **Means:** Shows median, spread (IQR), and outliers; can compare across categories.
- **Used:** Outlier detection, comparing a numeric variable across groups.
- **Use when:** Numeric variable split by a categorical variable.
- **Don't use when:** Comparing more than a handful of categories — gets cluttered.
- **Common mistake:** Not knowing the IQR rule (outliers = beyond 1.5×IQR from Q1/Q3) when asked to explain the whiskers.

### Bar plot vs count plot
- **Means:** `barplot` = aggregated statistic (e.g. mean) per category with error bars; `countplot` = raw frequency of each category.
- **Used:** `barplot` for "average X per category"; `countplot` for "how many rows per category."
- **Use when:** Choose based on whether you're aggregating a numeric column or just counting rows.
- **Don't use when:** Don't use `countplot` when you actually need an aggregated numeric value — it only counts rows.
- **Common mistake:** Mixing up the two and misreporting a count as an average, or vice versa.

### Scatter plots & relationship plots
- **Means:** Shows relationship between two numeric variables; `hue`/`size`/`col` extend to 3+ variables.
- **Used:** Exploring correlation/relationship before modeling.
- **Use when:** Two continuous variables, optionally colored/faceted by a third categorical variable.
- **Don't use when:** Very large N without adjusting for overplotting.
- **Common mistake:** Plotting a large dataset without `alpha` transparency or sampling, producing an unreadable solid blob.

### Correlation heatmap
- **Means:** Matrix view of pairwise correlations between numeric features.
- **Used:** Spotting multicollinearity before modeling.
- **Use when:** Numeric feature set, before feature selection.
- **Don't use when:** Categorical features (need encoding first) or too many features (unreadable matrix).
- **Common mistake:** Ignoring high correlation (|corr| > ~0.8) between two features feeding into a linear model.

### Multiple subplots (EDA dashboard)
- **Means:** Grid of related charts in one figure.
- **Used:** Compact, side-by-side EDA overview.
- **Use when:** Presenting several related views of the same dataset together.
- **Don't use when:** Charts aren't related/comparable — better as separate figures.
- **Common mistake:** Forgetting `plt.tight_layout()`, causing overlapping titles/labels.

### Plotting missing values
- **Means:** Visualizing where and how much data is missing.
- **Used:** Communicating data quality issues.
- **Use when:** Early EDA, before deciding on imputation strategy.
- **Don't use when:** Dataset has no missing values — skip.
- **Common mistake:** Reporting missingness only as a table, which is harder for stakeholders to parse quickly than a chart.

### Target analysis
- **Means:** Plotting the target variable's distribution (regression) or class balance (classification).
- **Used:** First plot before any modeling.
- **Use when:** Always, before building any model.
- **Don't use when:** N/A — always check the target first.
- **Common mistake:** Skipping this and discovering severe class imbalance only after training.

### Common plotting mistakes (checklist)
- **Means:** Recurring visualization errors.
- **Used:** Self-review before sharing a chart.
- **Use when:** Reviewing any chart before presenting it.
- **Don't use when:** N/A.
- **Common mistake:** No axis labels/title; wrong chart type for the data; truncated/misleading y-axis; overplotting; non-colorblind-safe palette for categories.
