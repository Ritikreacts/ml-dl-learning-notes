# pandas — Day 1 Notes

### Inspecting data (shape, dtypes, info, isna)
- **Means:** First-look checks on a new dataset.
- **Used:** Start of every EDA/project.
- **Use when:** Immediately after loading any new dataset.
- **Don't use when:** N/A — always do this first.
- **Common mistake:** Skipping straight to modeling without checking dtypes/missingness first.

### Selecting: `loc` vs `iloc`
- **Means:** `loc` = label-based selection (inclusive of slice end); `iloc` = position-based (exclusive of slice end).
- **Used:** Selecting rows/columns by name or by position.
- **Use when:** `loc` when you know column/index labels; `iloc` when working purely by position.
- **Don't use when:** Don't use `iloc` with labels or `loc` with integer positions — they don't mix.
- **Common mistake:** Expecting `loc` slices to exclude the endpoint like Python slicing does — they don't.

### Filtering rows
- **Means:** Boolean-mask row selection.
- **Used:** Subsetting data by condition(s).
- **Use when:** Combining conditions, use `&`/`|` with parentheses around each condition.
- **Don't use when:** Never use Python's `and`/`or` on a Series.
- **Common mistake:** Using `and`/`or` instead of `&`/`|`, raising a "truth value ambiguous" error.

### `apply` / `map` / `replace`
- **Means:** Element-wise or column-wise value transformation.
- **Used:** Custom cleaning logic, value lookups/renaming.
- **Use when:** No vectorized pandas/NumPy equivalent exists.
- **Don't use when:** A vectorized operation can do the same thing — `apply` loops in Python and is much slower.
- **Common mistake:** Reaching for `apply` by default instead of checking for a vectorized alternative first.

### Adding/dropping columns & rows
- **Means:** Modifying a DataFrame's shape/content.
- **Used:** Feature engineering, removing bad rows/columns.
- **Use when:** Prefer reassignment (`df = df.drop(...)`) for chainable code.
- **Don't use when:** Avoid `inplace=True` if you plan to chain further operations — it returns `None`.
- **Common mistake:** Chaining a method after `inplace=True` and getting `AttributeError: 'NoneType'`.

### GroupBy: `agg` vs `transform`
- **Means:** `agg` reduces to one row per group; `transform` returns a same-length Series broadcast back to the original shape.
- **Used:** Aggregated summaries (`agg`) vs. per-row features like "value minus group mean" (`transform`).
- **Use when:** `transform` when the result needs to stay aligned with the original DataFrame as a new column.
- **Don't use when:** Don't use `agg` when you need the result to fit back into the original row count.
- **Common mistake:** Using `agg` when a same-length column was actually needed, then struggling to merge it back.

### Joining: `merge` / `join` / `concat`
- **Means:** Combining tables on keys (`merge`), on index (`join`), or by stacking (`concat`).
- **Used:** Combining Olist-style multi-table data (orders, customers, reviews, etc.).
- **Use when:** Merge keys are checked for uniqueness first; correct `how` chosen based on which rows must not be lost.
- **Don't use when:** Don't merge blindly on a key without checking uniqueness on at least one side.
- **Common mistake:** Merging on a non-unique key on both sides, silently multiplying rows (many-to-many blowup). Always check `df.shape` before/after.

### Reshaping: `pivot` / `pivot_table` / `melt`
- **Means:** Long-to-wide (`pivot`/`pivot_table`) or wide-to-long (`melt`) transformations.
- **Used:** Building summary tables, preparing data for certain plots/models.
- **Use when:** `pivot_table` when duplicate index/column combinations exist and need aggregation.
- **Don't use when:** Plain `pivot` when duplicates exist — it will error.
- **Common mistake:** Using `pivot` instead of `pivot_table` and hitting a duplicate-entry error.

### Date/time operations
- **Means:** Parsing and deriving features from timestamps.
- **Used:** Delivery delay, cohort month, time-based features.
- **Use when:** Convert to `datetime` immediately after loading (`pd.to_datetime`) before any date math.
- **Don't use when:** Don't do date math on string-typed date columns.
- **Common mistake:** Forgetting to convert to `datetime`, causing silent string-based sorting/comparison bugs.

### Missing values
- **Means:** Detecting and handling `NaN`s.
- **Used:** Every real dataset.
- **Use when:** Median imputation for skewed data; mean for roughly symmetric data; `dropna` when the column is critical and rarely missing.
- **Don't use when:** Don't drop rows/columns with missing values without checking how much data that costs.
- **Common mistake:** Imputing with mean on a heavily skewed column, distorting the distribution.

### Duplicates
- **Means:** Identical rows or repeated keys.
- **Used:** Data quality checks before joins/aggregation.
- **Use when:** Check `duplicated()` before merges and before groupby summaries.
- **Don't use when:** Don't drop duplicates without checking if they're legitimately repeated events vs. true dupes.
- **Common mistake:** Not checking for duplicate keys before a merge, causing row inflation.

### Copy vs view / chained indexing
- **Means:** pandas doesn't guarantee whether a slice is a view or a copy; chained indexing (`df[cond]["col"] = x`) can silently fail to update the original.
- **Used:** Any conditional value assignment.
- **Use when:** Always use single-step `.loc[cond, "col"] = value`.
- **Don't use when:** Never chain two indexing operations for assignment.
- **Common mistake:** `df[df.a > 0]["b"] = 1` — triggers `SettingWithCopyWarning` and may not update `df`.

### Ranking within groups
- **Means:** Assigning a rank to rows within each group.
- **Used:** "Top-N per group" tasks (e.g., top product per category).
- **Use when:** `groupby().rank()` for a ranked column; `groupby().apply(lambda g: g.nlargest(n, col))` for extracting top-N rows.
- **Don't use when:** Don't use plain `sort_values()` alone if you need ranks reset per group.
- **Common mistake:** Sorting the whole DataFrame and assuming rank resets per group automatically — it doesn't.

### Cohort analysis
- **Means:** Grouping customers by first-purchase month, tracking behavior in later months.
- **Used:** Retention/churn analysis.
- **Use when:** You need to study behavior relative to a customer's starting point, not calendar time.
- **Don't use when:** Not needed for cross-sectional (single-snapshot) analysis.
- **Common mistake:** Using order date directly instead of the customer's first-order month, conflating cohort effects with calendar effects.
