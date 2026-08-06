# SciPy (stats) — Day 2 Notes

### Z-score (`stats.zscore`)
- **Means:** How many standard deviations a value is from the mean.
- **Used:** Outlier detection — flag values with `|z| > 3`.
- **Use when:** Data is roughly normal-shaped and you want a standardized way to spot extremes.
- **Don't use when:** Data is heavily skewed — extreme skew distorts the mean/std, making z-scores misleading. Use IQR method instead.
- **Common mistake:** Assuming z-score outliers = errors in the data; they might just be genuine extreme values.

### Skewness (`stats.skew`)
- **Means:** The gap between mean and median caused by extreme values pulling one direction. Mean > median = positive skew (right tail); mean < median = negative skew (left tail).
- **Used:** Deciding whether a feature needs transforming (e.g. log transform) before feeding it to a model.
- **Use when:** Checking distribution shape of numeric columns during EDA.
- **Don't use when:** Skew near 0 — data's already roughly symmetric, no transform needed.
- **Common mistake:** Confusing skew (direction of the pull) with kurtosis (how heavy the tails are) — they measure different things.

### Kurtosis (`stats.kurtosis`)
- **Means:** How often extreme/outlier values show up compared to a normal distribution (fatness of the tails).
- **Used:** Gauging how outlier-prone a numeric feature is.
- **Use when:** Combined with skew during EDA to fully describe a distribution's shape.
- **Don't use when:** Small sample sizes — kurtosis estimates get unstable with too few data points.
- **Common mistake:** Reading high kurtosis as "high spread" — it's about tail extremity, not overall spread (that's std).

### Standard Error (`stats.sem`)
- **Means:** How much a sample average would vary/wobble if you took different samples from the same population.
- **Used:** Foundation for building confidence intervals.
- **Use when:** You have one sample and want to estimate the reliability of its mean.
- **Don't use when:** You already have the full population — standard error is a sampling concept, not needed on complete data.
- **Common mistake:** Confusing standard error (variability of the *average*) with standard deviation (variability of *individual* values).

### Confidence Interval (`stats.t.interval`)
- **Means:** A range around the sample mean where the true population mean likely falls (e.g. 95% CI).
- **Used:** Communicating uncertainty around an estimate, not just a single number.
- **Use when:** Sample size is small or population std is unknown — use `t.interval` (not `norm.interval`).
- **Don't use when:** Interpreting it as "95% of individual values fall in this range" — it's about the *mean*, not individual data points.
- **Common mistake:** Misreading "95% confidence" as "95% probability the true mean is in this exact range" (subtly wrong interpretation, but commonly used loosely).

### Hypothesis Testing — Null vs Alternative
- **Means:** Null hypothesis (H₀) = no real difference/effect exists; Alternative (H₁) = a real difference exists. You start by assuming H₀ is true.
- **Used:** Framework for deciding whether an observed difference (e.g. churned vs non-churned tenure) is real or random noise.
- **Use when:** Comparing groups and needing a statistically grounded decision, not just eyeballing the numbers.
- **Don't use when:** You already have full population data — hypothesis testing is for inferring from samples.
- **Common mistake:** Treating "fail to reject H₀" as "H₀ is proven true" — it just means there wasn't enough evidence against it.

### P-value
- **Means:** Probability of seeing a difference this large (or larger) if H₀ were actually true.
- **Used:** Deciding significance — common cutoff is p < 0.05 → reject H₀.
- **Use when:** Interpreting the output of any statistical test (`ttest_ind`, `chi2_contingency`, etc.).
- **Don't use when:** Trying to judge effect size or practical importance — p-value doesn't measure that.
- **Common mistake:** Reading p-value as "probability H₀ is true" or "probability the result is due to chance" — both are wrong; it's a conditional probability *assuming* H₀ is true.

### Two-sample t-test (`stats.ttest_ind`)
- **Means:** Compares the means of a numeric variable across two groups, returns a t-statistic and p-value.
- **Used:** Day 2 daily task — comparing tenure (or similar numeric column) between churned vs non-churned customers.
- **Use when:** Comparing a continuous variable across exactly two independent groups.
- **Don't use when:** Comparing two categorical variables (e.g. churn vs contract type) — use `chi2_contingency` instead.
- **Common mistake:** Running the test and stopping at "p < 0.05 = significant" without stating what the result does and doesn't allow you to claim (no causation, no effect size).
