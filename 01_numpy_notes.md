# NumPy — Day 1 Notes

### Arrays & dtype
- **Means:** Fixed-type, contiguous array; faster than Python lists.
- **Used:** Any numerical data storage/computation in ML.
- **Use when:** Numeric data with a single, known type.
- **Don't use when:** Mixed types or need labeled columns → use pandas instead.
- **Common mistake:** Mixing types (e.g. `[1, "a", 2]`) silently upcasts everything to strings.

### arange / linspace / zeros / ones / eye / random
- **Means:** Structured array generation without typing every value.
- **Used:** Building ranges, placeholders, identity matrices, random init.
- **Use when:** `arange` for step-based ranges; `linspace` when you need an exact count of points.
- **Don't use when:** `arange` with float steps and count matters — rounding errors change the length.
- **Common mistake:** Forgetting `np.random.seed()`, making results non-reproducible.

### Reshape / transpose / ravel vs flatten / stack / split
- **Means:** Changing the shape/orientation of data without changing values.
- **Used:** Prepping arrays for model input shapes, combining/splitting batches.
- **Use when:** Total element count matches the target shape (`reshape(-1, 1)` common for sklearn targets).
- **Don't use when:** You need full independence from the source array — then use `flatten()`/`.copy()`, not `ravel()`.
- **Common mistake:** Modifying a `ravel()` result and being surprised the original array also changed (it's a view).

### Indexing, slicing, boolean indexing, `np.where`
- **Means:** Selecting values by position, range, or condition.
- **Used:** Filtering, conditional replacement, one-hot encoding.
- **Use when:** Boolean masks for condition-based selection; `np.where` for conditional value replacement.
- **Don't use when:** You need to preserve a live link to the original array — boolean indexing always returns a copy.
- **Common mistake:** Assuming boolean-indexed results are views; they aren't.

### Vectorization & broadcasting
- **Means:** Replacing explicit loops with array-level ops; broadcasting lets differently-shaped arrays combine by aligning shapes from the right.
- **Used:** Any bulk numeric operation — distance calculations, normalization, batch math.
- **Use when:** Shapes are equal, or one dimension is 1.
- **Don't use when:** Shapes genuinely mismatch — will raise an error; fix with `reshape(-1,1)` or `[:, None]`.
- **Common mistake:** Forgetting to add a dimension before combining a (n,) array with a (n,m) array, causing shape errors.

### Aggregation, axis, keepdims
- **Means:** Reducing an array along a dimension.
- **Used:** Column/row sums, means, normalization.
- **Use when:** `axis=0` to collapse rows (per-column result), `axis=1` to collapse columns (per-row result); `keepdims=True` when the result must broadcast back.
- **Don't use when:** Don't skip `keepdims` if you plan to subtract/divide the result back into the original array.
- **Common mistake:** Mixing up `axis=0` vs `axis=1`.

### Dot product / matrix multiplication
- **Means:** `@` / `np.dot` for matrix multiplication.
- **Used:** Core of every neural network forward pass (`X @ W + b`).
- **Use when:** Shapes are compatible for matrix multiplication (inner dimensions match).
- **Don't use when:** Element-wise multiplication is needed — use `*`, not `@`.
- **Common mistake:** Confusing `*` (element-wise) with `@` (matrix multiply).

### Sigmoid / softmax / MSE from scratch
- **Means:** Core ML math functions implemented manually.
- **Used:** Building blocks of logistic regression, classification outputs, regression loss.
- **Use when:** Understanding/debugging what a library function does internally.
- **Don't use when:** In production — use library implementations (numerically hardened, faster).
- **Common mistake:** Not subtracting the max before `np.exp()` in softmax, causing overflow.

### Views vs copies
- **Means:** Slicing returns a view (shared memory); fancy/boolean indexing returns a copy.
- **Used:** Any time you edit a subset of an array.
- **Use when:** Use `.copy()` explicitly if you need the subset to be independent.
- **Don't use when:** Don't assume all indexing behaves the same way — it doesn't.
- **Common mistake:** Editing a view, expecting the original to be unaffected.
