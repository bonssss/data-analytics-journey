# 🔢 NumPy Complete Guide & Cheatsheet

NumPy (**Numerical Python**) is the foundational building block of the entire Python scientific and data analysis ecosystem. Libraries like **pandas**, **scikit-learn**, and **matplotlib** are built directly on top of NumPy arrays.

---

## 📑 Table of Contents
- [1. Why NumPy? (vs. Python Lists)](#1-why-numpy-vs-python-lists)
- [2. Array Creation & Initializers](#2-array-creation--initializers)
- [3. Array Attributes & Data Types](#3-array-attributes--data-types)
- [4. Reshaping: Reshape, Ravel, and Flatten](#4-reshaping-reshape-ravel-and-flatten)
- [5. Vectorized Arithmetic & Broadcasting](#5-vectorized-arithmetic--broadcasting)
- [6. Universal Functions (ufuncs)](#6-universal-functions-ufuncs)
- [7. Indexing, Slicing & Boolean Masking](#7-indexing-slicing--boolean-masking)
- [8. Views vs. Copies (Critical Concept!)](#8-views-vs-copies-critical-concept)
- [9. Stacking & Matrix Operations](#9-stacking--matrix-operations)
- [10. Aggregations & Axis Operations](#10-aggregations--axis-operations)
- [11. Custom Function Vectorization](#11-custom-function-vectorization)
- [12. NumPy Cheatsheet Reference Table](#12-numpy-cheatsheet-reference-table)

---

## 1. Why NumPy? (vs. Python Lists)

| Feature | Python List | NumPy `ndarray` |
| :--- | :--- | :--- |
| **Memory Storage** | Pointers to scattered heap objects | Contiguous, dense block of memory |
| **Data Types** | Heterogeneous (can mix types) | Homogeneous (single fixed type) |
| **Speed** | Slow due to dynamic type-checking | Written in C; near hardware-level speed |
| **Vectorization** | Requires explicit `for` loops | Native element-wise operations |

```python
# In standard Python (Slow):
squares = [x**2 for x in range(1_000_000)]

# In NumPy (10x-50x Faster):
arr = np.arange(1_000_000)
squares = arr ** 2
```

---

## 2. Array Creation & Initializers

```python
import numpy as np

# From existing Python lists
a1 = np.array([1, 2, 3])              # 1D array
a2 = np.array([[1, 2, 3], [4, 5, 6]]) # 2D matrix (2 rows, 3 cols)

# Built-in Generators
np.zeros((3, 3))          # 3x3 array of zeros
np.ones((2, 4))           # 2x4 array of ones
np.empty((2, 3))          # Uninitialized memory (fastest allocation)
np.arange(0, 10, 2)       # [0, 2, 4, 6, 8] (start, stop, step)
np.linspace(0, 1, 5)      # 5 evenly spaced numbers from 0 to 1
np.logspace(1, 3, 3)      # 10^1, 10^2, 10^3 -> [10., 100., 1000.]
```

---

## 3. Array Attributes & Data Types

Every NumPy array has core inspection attributes:

```python
arr = np.array([[10, 20, 30], [40, 50, 60]], dtype=np.int32)

print(arr.ndim)      # 2 (Number of dimensions / axes)
print(arr.shape)     # (2, 3) (2 rows, 3 columns)
print(arr.size)      # 6 (Total number of elements)
print(arr.dtype)     # int32 (Data type of elements)
print(arr.itemsize)  # 4 (Bytes per element)

# Type casting:
float_arr = arr.astype(np.float64)  # Converts to float
```

---

## 4. Reshaping: Reshape, Ravel, and Flatten

Changing array dimensions without altering the underlying data:

```python
arr = np.arange(6) # array([0, 1, 2, 3, 4, 5])

# 1. Reshape into 2 rows, 3 columns
matrix = arr.reshape(2, 3)

# 2. Reshape with inferred dimension (-1 automatically computes size)
matrix = arr.reshape(3, -1) # (3 rows, 2 cols automatically)

# 3. Ravel vs Flatten (Returning to 1D)
r = matrix.ravel()    # Returns a VIEW (fast, mutating r mutates matrix!)
f = matrix.flatten()  # Returns a COPY (safe, separate copy in memory)
```

---

## 5. Vectorized Arithmetic & Broadcasting

### Element-Wise Arithmetic
Arithmetic operators (`+`, `-`, `*`, `/`, `**`, `//`, `%`) automatically apply to each corresponding element:

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

print(a + b)  # [11, 22, 33]
print(a * b)  # [10, 40, 90]
print(a ** 2) # [1, 4, 9]
```

### Broadcasting Rules
NumPy allows arithmetic between arrays of different shapes if their dimensions are compatible (matching from right to left, or one of them equals 1):

```
Matrix (3, 3):     Scalar (1):       Result (3, 3):
[ [1, 2, 3],                         [ [11, 12, 13],
  [4, 5, 6],   +       10       =      [14, 15, 16],
  [7, 8, 9] ]                          [17, 18, 19] ]
```

---

## 6. Universal Functions (ufuncs)

Fast, element-wise vectorized mathematical functions:

```python
arr = np.array([1, 4, 9, 16])

np.sqrt(arr)         # [1., 2., 3., 4.]
np.exp([1, 2])       # Exponential e^x
np.log(arr)          # Natural logarithm
np.sin(arr)          # Trigonometric sine
np.maximum(a, b)     # Element-wise maximum between two arrays
```

---

## 7. Indexing, Slicing & Boolean Masking

### 1D & 2D Slicing
```python
matrix = np.array([
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
])

# matrix[row_slice, col_slice]
matrix[0, 1]       # 20 (row 0, col 1)
matrix[0:2, 1:3]   # Sub-matrix: rows 0-1, cols 1-2
matrix[:, 0]       # First column as a 1D vector: [10, 40, 70]
matrix[1, :]       # Second row: [40, 50, 60]
```

### Boolean Filtering & Masking
```python
nums = np.array([12, 45, 67, 23, 89, 34])

# Boolean mask
mask = (nums > 30) & (nums < 80)  # Logical AND uses '&', NOT 'and'
print(nums[mask])                 # [45, 67, 34]

# Find indices matching condition:
indices = np.argwhere(nums > 50)
```

---

## 8. Views vs. Copies (Critical Concept!)

> [!WARNING]
> Basic slicing in NumPy returns a **view**, not a copy! Modifying a slice directly alters the original array!

```python
original = np.array([1, 2, 3, 4, 5])
view_slice = original[1:4] # View of [2, 3, 4]

view_slice[0] = 999
print(original)  # [1, 999, 3, 4, 5] -> Original was altered!

# Check if an array is a view or owns its memory:
print(view_slice.base is original) # True

# Safe copying:
safe_copy = original[1:4].copy()
safe_copy[0] = 42
print(original)  # Original remains untouched!
```

---

## 9. Stacking & Matrix Operations

```python
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])

# Horizontal stack
np.hstack((arr1, arr2)) # [1, 2, 3, 4, 5, 6]

# Vertical stack
np.vstack((arr1, arr2)) 
# [[1, 2, 3],
#  [4, 5, 6]]

# Transpose
matrix = np.array([[1, 2], [3, 4], [5, 6]])
print(matrix.T)          # Shape becomes (2, 3)
```

---

## 10. Aggregations & Axis Operations

> [!TIP]
> - `axis=0`: Collapse rows (aggregate **down each column**).
> - `axis=1`: Collapse columns (aggregate **across each row**).

```python
matrix = np.array([
    [10, 20, 30],
    [40, 50, 60]
])

print(matrix.sum())            # 210 (overall sum)
print(matrix.sum(axis=0))      # [50, 70, 90] (column sums)
print(matrix.sum(axis=1))      # [60, 150] (row sums)
print(matrix.mean(axis=0))     # Column averages
print(matrix.std())            # Standard deviation
print(matrix.min(), matrix.max()) # Min & Max
```

---

## 11. Custom Function Vectorization

When a Python function does not natively support NumPy arrays, use `np.vectorize`:

```python
def categorize_score(score):
    if score >= 90:
        return 'A'
    elif score >= 75:
        return 'B'
    else:
        return 'C'

v_categorize = np.vectorize(categorize_score)
scores = np.array([95, 82, 60, 88])
print(v_categorize(scores)) # ['A', 'B', 'C', 'B']
```

---

## 12. NumPy Cheatsheet Reference Table

| Category | Method | Description |
| :--- | :--- | :--- |
| **Creation** | `np.array(seq)` | Convert list/tuple into ndarray |
| | `np.zeros(shape)` | Array filled with 0.0 |
| | `np.ones(shape)` | Array filled with 1.0 |
| | `np.arange(start, stop, step)` | Step-based range array |
| | `np.linspace(start, stop, n)` | Evenly spaced points array |
| **Shapes** | `arr.shape` | Tuple of array dimensions |
| | `arr.reshape(r, c)` | Change dimensions without data copy |
| | `arr.ravel()` | Flatten to 1D view |
| | `arr.flatten()` | Flatten to 1D deep copy |
| **Math** | `np.sum(arr, axis)` | Sum of array elements |
| | `np.mean(arr, axis)` | Mean of array elements |
| | `np.std(arr, axis)` | Standard deviation |
| | `np.sqrt(arr)`, `np.exp(arr)` | Fast math ufuncs |
| **Inspection** | `arr.dtype` | Element data type |
| | `arr.ndim` | Number of array dimensions |
| | `arr.size` | Total element count |

---

[⬅️ Back to README](../README.md) | [Next: Pandas Guide ➡️](./pandas_guide.md)
