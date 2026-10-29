# 🐼 Pandas Complete Guide & Cheatsheet

**pandas** is the core tool in Python for tabular data manipulation, exploratory data analysis (EDA), data wrangling, and cleaning. It provides intuitive data structures—`Series` and `DataFrame`—built on top of high-performance NumPy arrays.

---

## 📑 Table of Contents
- [1. Core Data Structures: Series vs DataFrame](#1-core-data-structures-series-vs-dataframe)
- [2. Inspecting DataFrames](#2-inspecting-dataframes)
- [3. Indexing & Selection (`loc` vs `iloc`)](#3-indexing--selection-loc-vs-iloc)
- [4. Conditional Filtering & Logical Operators](#4-conditional-filtering--logical-operators)
- [5. Column & Row Operations](#5-column--row-operations)
- [6. Data Cleaning & Handling Missing Values](#6-data-cleaning--handling-missing-values)
- [7. Combining Datasets: Concat & Merge](#7-combining-datasets-concat--merge)
- [8. Loading & Exporting Data](#8-loading--exporting-data)
- [9. Pandas Cheatsheet Reference Table](#9-pandas-cheatsheet-reference-table)

---

## 1. Core Data Structures: Series vs DataFrame

| Structure | Dimensions | Analogy | Description |
| :--- | :--- | :--- | :--- |
| **`pd.Series`** | 1D | Single spreadsheet column | Homogeneous 1D labeled array with an index |
| **`pd.DataFrame`** | 2D | Entire spreadsheet table | 2D labeled tabular structure with rows and columns |

```python
import pandas as pd

# Creating a Series
salaries = pd.Series([50000, 65000, 72000], index=['Alice', 'Bob', 'Charlie'], name='Salary')
print(salaries['Alice']) # 50000

# Creating a DataFrame from a dictionary
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Department': ['IT', 'HR', 'IT', 'Finance'],
    'Salary': [75000, 52000, 81000, 64000],
    'YearsExp': [3, 5, 7, 2]
}
df = pd.DataFrame(data)
```

---

## 2. Inspecting DataFrames

When loading any dataset, always run these initial inspection commands:

```python
df.head(5)       # Preview first 5 rows
df.tail(3)       # Preview last 3 rows
df.shape         # (rows, columns) tuple: e.g., (1000, 12)
df.columns       # List of column names
df.dtypes        # Data type of each column

df.info()        # Summary of columns, non-null counts, and memory usage
df.describe()    # Descriptive stats for numerical columns (count, mean, std, min, 25%, 50%, 75%, max)
```

---

## 3. Indexing & Selection (`loc` vs `iloc`)

> [!IMPORTANT]
> - **`iloc`** = **Integer-location based**: uses 0-indexed integer positions (like standard Python lists).
> - **`loc`** = **Label based**: uses row index labels and column names.

```python
# Select single column (returns a Series)
df['Salary']

# Select multiple columns (returns a DataFrame)
df[['Name', 'Salary']]

# iloc: [row_position, column_position]
df.iloc[0, 1]          # Row 0, Column 1
df.iloc[0:2, 0:3]      # Rows 0-1, Columns 0-2 (exclusive stop index)
df.iloc[[0, 2]]        # Specific rows at indices 0 and 2

# loc: [row_label, column_label]
df.loc[0, 'Salary']    # Row with label 0, column 'Salary'
df.loc[0:2, 'Name':'Salary'] # Note: 'loc' includes BOTH start and end boundaries!
```

---

## 4. Conditional Filtering & Logical Operators

In pandas, use bitwise operators instead of Python's `and`, `or`, `not`:
- `&` for **AND**
- `|` for **OR**
- `~` for **NOT**

Always wrap each condition in parentheses `(...)`:

```python
# Single condition: High earners
high_salary = df[df['Salary'] > 70000]

# Multiple conditions: IT department AND salary > 75000
it_high = df[(df['Department'] == 'IT') & (df['Salary'] > 75000)]

# Multiple conditions: HR OR Finance
hr_or_fin = df[(df['Department'] == 'HR') | (df['Department'] == 'Finance')]

# Using .isin() for cleaner multi-value matching:
target_depts = df[df['Department'].isin(['HR', 'Finance'])]

# Invert condition with ~
non_it = df[~(df['Department'] == 'IT')]
```

---

## 5. Column & Row Operations

### Adding & Modifying Columns (Broadcasting)
```python
# Broadcast arithmetic across all rows
df['Annual_Bonus'] = df['Salary'] * 0.10

# Categorical column assignment
df['Status'] = 'Active'
```

### Dropping Columns & Rows
```python
# Drop a column (axis=1 or columns=...)
df_clean = df.drop(columns=['Status'])

# Drop rows by index
df_dropped = df.drop(index=[0, 2])

# In-place modification:
df.drop(columns=['Annual_Bonus'], inplace=True)
```

---

## 6. Data Cleaning & Handling Missing Values

Real-world datasets are almost always messy with missing values (`NaN` / `None`).

### 1. Spotting Missing Values
```python
df.isnull().sum()  # Count of missing values per column
df.isna().mean()   # Percentage of missing values per column
```

### 2. Dropping Missing Values (`dropna`)
```python
# Drop rows where ANY column is NaN
df.dropna()

# Drop rows where ALL columns are NaN
df.dropna(how='all')

# Drop rows where specific columns are NaN
df.dropna(subset=['Salary', 'Department'])
```

### 3. Imputing Missing Values (`fillna`)
```python
# Fill with a constant value or statistical metric:
median_salary = df['Salary'].median()
df['Salary'] = df['Salary'].fillna(median_salary)

# Forward fill (propagate last valid observation forward):
df.ffill()

# Backward fill (propagate next valid observation backward):
df.bfill()
```

---

## 7. Combining Datasets: Concat & Merge

### Concatenation (`pd.concat`)
Used for stacking DataFrames vertically (union of rows) or horizontally (adding columns side-by-side):

```python
# Stacking rows (axis=0)
all_employees = pd.concat([df_branch_a, df_branch_b], axis=0, ignore_index=True)

# Stacking columns (axis=1)
full_profile = pd.concat([df_demographics, df_payroll], axis=1)
```

### Merging / Relational Joins (`pd.merge`)
Used for joining DataFrames based on common key columns (similar to SQL `JOIN`):

```python
# Location DataFrame
locations = pd.DataFrame({
    'Department': ['IT', 'HR', 'Finance'],
    'City': ['Seattle', 'Chicago', 'New York']
})

# Inner Join on 'Department'
merged = pd.merge(df, locations, on='Department', how='inner')

# Left Join (keep all rows from df, add City if found)
left_merged = pd.merge(df, locations, on='Department', how='left')
```

---

## 8. Loading & Exporting Data

### Loading CSV Files
```python
import os

# Robust relative path loading pattern:
data_path = "data/Housing.csv" if os.path.exists("data/Housing.csv") else "Housing.csv"
data = pd.read_csv(data_path)

# Useful parameters for read_csv:
# - sep=',' (delimiter)
# - encoding='utf-8' or 'latin-1' (character encoding)
# - usecols=['price', 'area'] (load only subset of columns for speed)
# - parse_dates=['date_column'] (auto-parse dates)
```

### Exporting Data
```python
# Export to CSV without saving the arbitrary integer row index
df.to_csv('cleaned_data.csv', index=False)
```

---

## 9. Pandas Cheatsheet Reference Table

| Category | Method | Description |
| :--- | :--- | :--- |
| **Inspection** | `df.head(n)` / `df.tail(n)` | View first/last $n$ records |
| | `df.info()` | Summary of column types and missing values |
| | `df.describe()` | Statistical distribution summary |
| | `df.shape` | `(n_rows, n_cols)` tuple |
| **Selection** | `df['col']` | Select column as Series |
| | `df[['c1', 'c2']]` | Select subset of columns as DataFrame |
| | `df.iloc[r, c]` | Integer-position based lookup |
| | `df.loc[r, c]` | Label-based lookup |
| **Filtering** | `df[(df['a'] > 0) & (df['b'] < 10)]` | Multi-condition boolean query |
| | `df[df['col'].isin(values)]` | Check membership in a list |
| **Cleaning** | `df.isnull().sum()` | Count NaNs per column |
| | `df.dropna(subset=[...])` | Remove rows with missing values |
| | `df.fillna(val)` | Replace NaNs with value or metric |
| | `df.drop_duplicates()` | Remove duplicate records |
| **Combine** | `pd.concat([d1, d2], axis=0)` | Stack rows together |
| | `pd.merge(d1, d2, on='key')` | SQL-style join on shared key |

---

[⬅️ NumPy Guide](./numpy_guide.md) | [Back to README](../README.md) | [Next: Matplotlib Guide ➡️](./matplotlib_guide.md)
