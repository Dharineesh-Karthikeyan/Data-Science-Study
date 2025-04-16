## 📋 Summary Table of Popular Pandas and NumPy Functions

| Library | Function                          | Purpose / Use Case                                                     |
|---------|-----------------------------------|------------------------------------------------------------------------|
| NumPy   | `np.array()`                      | Create a NumPy array                                                   |
| NumPy   | `np.arange()`, `np.linspace()`    | Generate evenly spaced values                                          |
| NumPy   | `np.reshape()`, `np.newaxis`      | Reshape arrays or add dimensions for broadcasting                      |
| NumPy   | `np.where()`                      | Vectorized conditional logic                                           |
| NumPy   | `np.select()`                     | Multi-condition vectorized logic                                       |
| NumPy   | `np.unique(return_counts=True)`   | Get unique values and their frequencies                               |
| NumPy   | `np.dot()`, `np.matmul()`         | Matrix multiplication                                                  |
| NumPy   | `np.linalg.inv()`, `eig()`, `svd()` | Inverse, eigenvalues, singular value decomposition                     |
| NumPy   | `np.isin()`                       | Check if elements are in another array                                 |
| NumPy   | `np.sum()`, `np.mean()`, `np.std()`| Basic statistics over arrays                                           |
| NumPy   | `np.random.rand()`, `np.random.randn()` | Generate random numbers                                           |
| NumPy   | `np.vectorize()`                  | Turn a function into a vectorized one                                  |
| Pandas  | `pd.Series()`, `pd.DataFrame()`   | Create Series or DataFrame                                             |
| Pandas  | `df.head()`, `df.tail()`          | Preview rows                                                           |
| Pandas  | `df.info()`, `df.describe()`      | Inspect metadata and summary stats                                     |
| Pandas  | `df.shape`, `df.columns`, `df.dtypes` | Basic DataFrame properties                                          |
| Pandas  | `df.loc[]`, `df.iloc[]`           | Label-based and position-based indexing                                |
| Pandas  | `df.at[]`, `df.iat[]`             | Fast access for single values                                          |
| Pandas  | `df.query()`                      | Filter rows using query strings                                        |
| Pandas  | `df.isnull()`, `df.fillna()`, `df.dropna()` | Handle missing values                                         |
| Pandas  | `df.duplicated()`, `df.drop_duplicates()` | Detect or drop duplicate rows                                   |
| Pandas  | `df.sort_values()`                | Sort rows by column values                                             |
| Pandas  | `df.set_index()`, `df.reset_index()` | Set or reset index                                                   |
| Pandas  | `df.astype()`                     | Change column data types                                               |
| Pandas  | `df.apply()`, `df.map()`, `df.applymap()` | Apply custom or element-wise functions                           |
| Pandas  | `df.groupby()`, `agg()`, `transform()` | Group and summarize/transform values                              |
| Pandas  | `pd.merge()`, `pd.concat()`, `df.join()` | Combine multiple DataFrames                                      |
| Pandas  | `df.pivot_table()`, `pd.crosstab()` | Summarize data in a matrix format                                  |
| Pandas  | `pd.cut()`, `pd.qcut()`           | Discretize continuous variables                                       |
| Pandas  | `df.sample()`                     | Randomly sample rows                                                  |
| Pandas  | `df.nunique()`                    | Count distinct values per column                                      |
| Pandas  | `df.memory_usage()`               | Inspect memory usage                                                  |
| Pandas  | `df.corr()`, `df.cov()`           | Correlation and covariance matrices                                   |
| Pandas  | `pd.to_datetime()`                | Convert strings to datetime                                            |
| Pandas  | `df.resample()`                   | Time-based resampling (e.g., daily → monthly)                         |
| Pandas  | `df.rolling()`, `df.expanding()`  | Moving window and expanding calculations                              |
| Pandas  | `df.plot()`                       | Quick plotting (requires matplotlib)                                  |


## 💬 Pandas & NumPy Interview Questions — Q&A

This section includes essential and advanced-level interview questions for Pandas and NumPy — formatted with simple explanations and Python examples.

---

### ❓ Q1: What’s the difference between `apply`, `map`, and `applymap` in Pandas?

**Answer**:

| Function     | Works On        | Description                                                              |
|--------------|-----------------|--------------------------------------------------------------------------|
| `map()`      | Series           | Element-wise transformation on a Series                                 |
| `apply()`    | Series/DataFrame | Applies a function across rows or columns                                |
| `applymap()` | DataFrame        | Element-wise function across all cells in a DataFrame                   |

```python
df['col'].map(lambda x: x + 1)
df.apply(lambda row: row.sum(), axis=1)
df.applymap(lambda x: x * 2)
```

---

### ❓ Q2: When to use `transform()` instead of `agg()` in Pandas?

**Answer**:

- `agg()` returns reduced output (e.g., one value per group).
- `transform()` returns an output with the **same shape** as the original DataFrame.

```python
df.groupby('group')['value'].agg('mean')
df['zscore'] = df.groupby('group')['value'].transform(lambda x: (x - x.mean()) / x.std())
```

---

### ❓ Q3: Why is vectorization preferred in NumPy?

**Answer**:

- Faster than loops (compiled C code).
- Uses less memory.
- Code is cleaner.

```python
np.square(np.array([1, 2, 3]))  # vectorized
```

---

### ❓ Q4: What is `SettingWithCopyWarning` and how to avoid it?

**Answer**:

This warning tells you that your operation might not affect the original DataFrame due to chained indexing.

✅ Use `.loc[]` to avoid it:
```python
df.loc[df['score'] > 80, 'status'] = 'Pass'
```

---

### ❓ Q5: How can you reduce memory usage in Pandas?

**Answer**:

- Downcast numeric types using `pd.to_numeric()`.
- Convert text columns to `category`.
- Use `df.memory_usage(deep=True)` to monitor.

```python
df['col'] = pd.to_numeric(df['col'], downcast='integer')
df['type'] = df['type'].astype('category')
```

---

### ❓ Q6: What is broadcasting in NumPy?

**Answer**:

Broadcasting allows arrays with different shapes to be combined in operations by automatically expanding them.

```python
a = np.array([[1], [2], [3]])  # shape (3,1)
b = np.array([10, 20, 30])     # shape (3,)
a + b  # becomes shape (3,3)
```

---

### ❓ Q7: What’s the difference between `merge()`, `concat()`, and `join()`?

**Answer**:

| Function   | Purpose                        |
|------------|--------------------------------|
| `merge()`  | SQL-style joins                |
| `concat()` | Combine rows/columns           |
| `join()`   | Join on index or key column    |

```python
pd.merge(df1, df2, on='id', how='left')
pd.concat([df1, df2], axis=1)
df1.join(df2.set_index('id'), on='id')
```

---

### ❓ Q8: How do you calculate a rolling average in Pandas?

**Answer**:

Use `.rolling()` with `.mean()`:
```python
df['7day_avg'] = df['value'].rolling(window=7).mean()
```

---

### ❓ Q9: How do you find the department with the highest average salary?

**Answer**:
```python
df[df['salary'] > 50000].groupby('department')['salary'].mean().idxmax()
```

---

### ❓ Q10: What’s the difference between `cut()` and `qcut()`?

**Answer**:

- `cut()`: Fixed value bins.
- `qcut()`: Quantile-based bins (equal-sized buckets).

```python
pd.cut(df['age'], bins=[0, 18, 35, 60])
pd.qcut(df['score'], q=4)  # quartiles
```

---

### ❓ Q11: How do you compute correlation or covariance?

**Answer**:

```python
df.corr()  # correlation
df.cov()   # covariance
```

---

### ❓ Q12: Difference between `np.dot()` and `np.matmul()`?

**Answer**:

- `np.dot()` handles 1D & 2D dot product.
- `np.matmul()` supports ND arrays and broadcasting.

```python
np.dot(A, B)
np.matmul(A, B)
```

---

### ❓ Q13: How to handle missing values in Pandas?

**Answer**:

```python
df.isnull().sum()
df.fillna(method='ffill')
df['col'].fillna(df['col'].mean(), inplace=True)
```

---

### ❓ Q14: What is `astype()` used for?

**Answer**:

Used to cast data to a specific type — often for memory optimization.

```python
df['col'] = df['col'].astype('int32')
df['col'] = df['col'].astype('category')
```

---

### ❓ Q15: How is `groupby().transform()` useful in feature engineering?

**Answer**:

It allows you to create **row-level group statistics**, like z-scores or group-level means.

```python
df['group_mean'] = df.groupby('group')['value'].transform('mean')
```

---

### ❓ Q16: Difference between `iloc[]` and `loc[]`?

**Answer**:

| Method   | Type      |
|----------|-----------|
| `iloc[]` | Integer position |
| `loc[]`  | Label/index name |

```python
df.iloc[0, 1]
df.loc[3, 'score']
```

---

### ❓ Q17: How to get frequency count of unique values?

**Answer**:
```python
df['col'].value_counts()
np.unique(arr, return_counts=True)
```

---

### ❓ Q18: When to use `np.select()` instead of `np.where()`?

**Answer**:

Use `np.select()` for **multiple** conditions.

```python
conditions = [arr > 90, arr > 75, arr > 50]
choices = ['A', 'B', 'C']
np.select(conditions, choices, default='F')
```

---

### ❓ Q19: What’s the use of `df.query()`?

**Answer**:

Cleaner filtering using strings:
```python
df.query("score > 80 and subject == 'Math'")
```

---

### ❓ Q20: How to perform one-hot encoding in Pandas?

**Answer**:
```python
pd.get_dummies(df, columns=['col'])
```

---

### ❓ Q21: What’s the use of `pivot_table()` and `crosstab()`?

**Answer**:

- `pivot_table()`: Group + aggregate + reshape
- `crosstab()`: Frequency table of 2 variables

```python
pd.pivot_table(df, values='sales', index='region', columns='product', aggfunc='sum')
pd.crosstab(df['gender'], df['purchased'])
```

---

