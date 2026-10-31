# 📊 Matplotlib Complete Guide & Visualization Handbook

Data visualization is one of the most critical skills for a data analyst. It allows you to explore raw data, uncover hidden anomalies, communicate findings clearly to non-technical stakeholders, and validate modeling assumptions.

---

## 📑 Table of Contents
- [1. Anatomy of a Matplotlib Figure](#1-anatomy-of-a-matplotlib-figure)
- [2. The Chart Selection Framework](#2-the-chart-selection-framework)
- [3. Line Plots: Trends & Formatting](#3-line-plots-trends--formatting)
- [4. Histograms: Frequency Distributions](#4-histograms-frequency-distributions)
- [5. Box Plots: Outlier Detection & Grouped Distributions](#5-box-plots-outlier-detection--grouped-distributions)
  - [Single-Variable Box Plot & Outlier Detection](#single-variable-box-plot--outlier-detection)
  - [Multi-Category Distribution Comparisons (Grouped Box Plots)](#multi-category-distribution-comparisons-grouped-box-plots)
  - [Crucial Parameter Gotcha: `tick_labels` vs `label`](#crucial-parameter-gotcha-tick_labels-vs-label)
  - [Idiomatic Alternative: Pandas `df.boxplot()`](#idiomatic-alternative-pandas-dfboxplot)
- [6. Categorical Visualization: Bar Charts vs. Pie Charts](#6-categorical-visualization-bar-charts-vs-pie-charts)
  - [Frequency Counts Comparison (`value_counts()`)](#frequency-counts-comparison-value_counts)
  - [Aggregated Metrics with GroupBy (`groupby().sum()`)](#aggregated-metrics-with-groupby-groupbysum)
  - [Pie Chart Proportions with Percentage Formatting](#pie-chart-proportions-with-percentage-formatting)
- [7. Scatter Plots: Bivariate Analysis & Correlation](#7-scatter-plots-bivariate-analysis--correlation)
- [8. Multivariate Visualizations: Adding Categorical Hue](#8-multivariate-visualizations-adding-categorical-hue)
  - [Visualizing 3 Dimensions in a 2D Scatter Plot](#visualizing-3-dimensions-in-a-2d-scatter-plot)
  - [Color Mapping & Dynamic Legends](#color-mapping--dynamic-legends)
- [9. The Object-Oriented (OO) API & Subplot Dashboards](#9-the-object-oriented-oo-api--subplot-dashboards)
  - [Pyplot Procedural Interface vs. Object-Oriented Architecture](#pyplot-procedural-interface-vs-object-oriented-architecture)
  - [Creating Multi-Panel Grids with `plt.subplots()`](#creating-multi-panel-grids-with-pltsubplots)
  - [Indexing Subplot Arrays (1D & 2D Grids)](#indexing-subplot-arrays-1d--2d-grids)
  - [Object-Oriented Method Mapping Reference](#object-oriented-method-mapping-reference)
  - [Layout Management with `tight_layout()`](#layout-management-with-tight_layout)
- [10. Essential Styling & Customization Tips](#10-essential-styling--customization-tips)
- [11. Matplotlib Cheatsheet Reference Table](#11-matplotlib-cheatsheet-reference-table)

---

## 1. Anatomy of a Matplotlib Figure

In Matplotlib, a plot consists of two foundational objects:
1. **`Figure`**: The top-level canvas / container holding all plot elements, subplots, legends, and canvas titles.
2. **`Axes`**: The actual plotting area with coordinate axes ($x$ and $y$), title, labels, ticks, and plotted data points.

```
+-----------------------------------------------------------+
| Figure                                                    |
|   +---------------------------------------------------+   |
|   | Axes (Title: "Salary Distribution")               |   |
|   |  Y-Axis                                           |   |
|   |    ^                                              |   |
|   |    |    *                                         |   |
|   |    |         *                                    |   |
|   |    |              *                               |   |
|   |    +------------------------> X-Axis              |   |
|   |      (Label: "Age")                               |   |
|   +---------------------------------------------------+   |
+-----------------------------------------------------------+
```

---

## 2. The Chart Selection Framework

| Analytical Goal | Recommended Chart | Matplotlib Function | Example Analytical Question |
| :--- | :--- | :--- | :--- |
| **Trend / Progression** | Line Plot | `plt.plot(x, y)` | How does average home price change over years? |
| **Distribution / Spread** | Histogram | `plt.hist(data, bins)` | How are restaurant food prices distributed? |
| **Outlier Detection** | Box Plot | `plt.boxplot(data)` | Are there extreme executive salaries skewing the mean? |
| **Group Comparison (Spread)** | Grouped Box Plot | `plt.boxplot([g1, g2], tick_labels=...)` | How do salaries vary across IT, Marketing, and Law? |
| **Category Comparison (Count)** | Bar Chart | `plt.bar(labels, values)` | Which department has the largest headcount? |
| **Aggregated Metric by Group** | Bar Chart / Pie Chart | `plt.bar(grp.index, grp.values)` | What is the total salary budget allocated to each department? |
| **Part-to-Whole Ratio** | Pie Chart / Donut | `plt.pie(values, labels=..., autopct='%1.1f%%')` | What percentage share of total budget does each department consume? |
| **Relationship / Correlation** | Scatter Plot | `plt.scatter(x, y)` | Does employee age correlate with salary? |
| **Multivariate (2 Num + 1 Cat)** | Colored Scatter Plot | `plt.scatter(x, y, color=...)` (per category) | How does age correlate with salary within each department? |
| **Multi-Panel Storytelling** | Subplots Grid | `fig, axs = plt.subplots(r, c)` | How can we present trends, distributions, and outliers together? |

---

## 3. Line Plots: Trends & Formatting

Used to show continuous movement or sorted relationships:

```python
import matplotlib.pyplot as plt
import pandas as pd

# Basic line plot with formatting
x = [1, 4, 5]
y = [2, 4, 5]

plt.plot(x, y, color="blue", linestyle="--", marker="o", linewidth=2)
plt.title("Sample Trend Line")
plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.grid(True)
plt.show()
```

> [!TIP]
> **Avoid Zigzag Crisscrosses**: When plotting a line graph against a continuous column (like `Age`), always sort the DataFrame by that column first (`sort_age = df.sort_values('Age')`), otherwise the lines will zigzag backward across the plot! Ensure you use `sort_age['Salary']` instead of `df['Salary']` so the $x$ and $y$ indices stay perfectly aligned.

---

## 4. Histograms: Frequency Distributions

Histograms divide continuous numerical data into discrete bins and count how many observations fall into each bin:

```python
# df['Salary'] distribution with 5 bins
plt.hist(df['Salary'], bins=5, color='teal', edgecolor='black')
plt.title("Salary Frequency Distribution")
plt.xlabel("Salary Range ($)")
plt.ylabel("Frequency (Count)")
plt.show()
```

- **Right-skewed (positive)**: Tail extends toward higher values (typical for income, house prices).
- **Left-skewed (negative)**: Tail extends toward lower values (typical for test scores).
- **Symmetric / Bell-shaped**: Data is evenly distributed around the mean.

---

## 5. Box Plots: Outlier Detection & Grouped Distributions

A Box Plot displays the **5-Number Summary**:
1. **Minimum** ($Q1 - 1.5 \times IQR$)
2. **First Quartile (Q1 / 25th percentile)**
3. **Median (Q2 / 50th percentile)**
4. **Third Quartile (Q3 / 75th percentile)**
5. **Maximum** ($Q3 + 1.5 \times IQR$)

Any points outside the whiskers are flagged as **outliers** (circles/dots).

### Single-Variable Box Plot & Outlier Detection

```python
plt.boxplot(df['Salary'])
plt.title("Salary Box Plot - Detecting Outliers")
plt.ylabel("Salary ($)")
plt.show()
```

#### Handling Outliers:
```python
# 1. Identify outlier record
outlier_index = df[df['Salary'] > 20000].index

# 2. Drop the anomaly to restore scale
df_clean = df.drop(index=outlier_index)
```

### Multi-Category Distribution Comparisons (Grouped Box Plots)

When comparing a continuous variable (e.g. `Salary`) across multiple discrete groups (e.g. `Dept`), pass a list of Series/arrays into `plt.boxplot()`:

```python
# Slicing values for each category
it_dep = df[df['Dept'] == "IT"]['Salary']
bus_dep = df[df['Dept'] == "Business"]['Salary']
mar_dep = df[df['Dept'] == "Marketing"]['Salary']
law_dep = df[df['Dept'] == "Law"]['Salary']
man_dep = df[df['Dept'] == "Management"]['Salary']

# Plotting all categories side-by-side
plt.figure(figsize=(9, 5))
plt.boxplot(
    [it_dep, bus_dep, mar_dep, law_dep, man_dep],
    tick_labels=["IT", "Business", "Marketing", "Law", "Management"]
)
plt.title("Department Salary Distribution Comparison")
plt.xlabel("Departments")
plt.ylabel("Salary ($)")
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
```

### Crucial Parameter Gotcha: `tick_labels` vs `label`

> [!CAUTION]
> **Do not use `label=[...]` in `plt.boxplot()`!**
> In Matplotlib:
> - `label=` (singular) is an `Artist` attribute intended for legends. When passed a list, it does **not** label the category tick marks on the x-axis, leaving the x-ticks as default numbers `1, 2, 3, 4, 5`.
> - Use **`tick_labels=[...]`** (Matplotlib 3.9+) or **`labels=[...]`** (earlier versions) to correctly assign category names along the x-axis.

### Idiomatic Alternative: Pandas `df.boxplot()`

Instead of manually filtering 5 individual slices, you can write concise, dynamic code that automatically accommodates any number of departments:

```python
# Method A: Automatic Pandas grouping (One-Liner)
df.boxplot(column='Salary', by='Dept', grid=False, figsize=(9, 5))
plt.title("Department Distribution Comparison")
plt.suptitle("") # Clears default pandas auto-title
plt.xlabel("Departments")
plt.ylabel("Salary ($)")
plt.show()

# Method B: Vectorized list comprehension in pure Matplotlib
depts = df['Dept'].unique()
dept_salaries = [df[df['Dept'] == d]['Salary'] for d in depts]

plt.boxplot(dept_salaries, tick_labels=depts)
plt.title("Department Distribution Comparison")
plt.xlabel("Departments")
plt.ylabel("Salary ($)")
plt.show()
```

---

## 6. Categorical Visualization: Bar Charts vs. Pie Charts

### Frequency Counts Comparison (`value_counts()`)
Bar charts provide an unambiguous comparison across categories because humans judge lengths much more accurately than angles:

```python
counts = df['Dept'].value_counts()

plt.bar(counts.index, counts.values, color='coral', edgecolor='black')
plt.title("Employee Count by Department")
plt.xlabel("Department")
plt.ylabel("Number of Employees")
plt.xticks(rotation=45)
plt.show()
```

### Aggregated Metrics with GroupBy (`groupby().sum()`)
Often you need to visualize aggregated numeric metrics (e.g. Total Salary Budget, Average Revenue) rather than simple frequency headcounts:

```python
# 1. Aggregate total salary spend by department
salary_by_dept = df.groupby("Dept")['Salary'].sum()

# 2. Visualize aggregated totals with a bar chart
plt.figure(figsize=(8, 4))
plt.bar(salary_by_dept.index, salary_by_dept.values, color='steelblue', edgecolor='black')
plt.title("Total Salary Expenditure by Department")
plt.xlabel("Department")
plt.ylabel("Total Expenditure ($)")
plt.show()
```

### Pie Chart Proportions with Percentage Formatting
Pie charts show part-to-whole percentage contributions. Always format percentages using `autopct="%1.1f%%"` so the percent sign (`%`) renders:

```python
plt.figure(figsize=(6, 6))
plt.pie(
    salary_by_dept,
    labels=salary_by_dept.index,
    autopct="%1.1f%%", # Display percentage with one decimal and '%' symbol
    startangle=140,
    colors=['#4E79A7', '#F28E2B', '#E15759', '#76B7B2', '#59A14F']
)
plt.title("Share of Total Salary Budget by Department")
plt.show()
```

> [!TIP]
> Use `autopct="%1.1f%%"` instead of `autopct="%1.1f"`. The trailing `%%` escapes and prints the literal `%` symbol on each slice.

---

## 7. Scatter Plots: Bivariate Analysis & Correlation

Scatter plots reveal whether an increase in variable $X$ is associated with an increase, decrease, or no change in variable $Y$:

```python
plt.scatter(df['Age'], df['Salary'], color='navy', alpha=0.7, s=60)
plt.title("Relationship: Age vs. Salary")
plt.xlabel("Age (Years)")
plt.ylabel("Salary ($)")
plt.grid(True, linestyle=":")
plt.show()
```

- **Positive Correlation**: Points slope upward from left to right.
- **Negative Correlation**: Points slope downward from left to right.
- **No Correlation**: Points appear as a random cloud without a clear directional slope.

---

## 8. Multivariate Visualizations: Adding Categorical Hue

### Visualizing 3 Dimensions in a 2D Scatter Plot
In data analytics, you often need to analyze the relationship between two numerical variables (`Age` and `Salary`) while grouping by a categorical dimension (`Dept`):
- **Dimension 1 (X-Axis)**: Age (Numerical)
- **Dimension 2 (Y-Axis)**: Salary (Numerical)
- **Dimension 3 (Color / Hue)**: Department (Categorical)

### Color Mapping & Dynamic Legends

To color-code points by category in pure Matplotlib, iterate through the categories and assign custom colors:

```python
# 1. Define distinct, accessible color palette for each category
colors = {
    "IT": "#1f77b4",          # Steel Blue
    "Management": "#8c564b",  # Brown
    "Law": "#bcbd22",         # Olive / Golden Yellow
    "Business": "#ff7f0e",    # Orange
    "Marketing": "#d62728"    # Crimson Red
}

plt.figure(figsize=(8, 5))

# 2. Iterate and plot each department as an independent scatter series
for dept, color in colors.items():
    data = df[df["Dept"] == dept]
    plt.scatter(
        data["Age"],
        data["Salary"],
        color=color,
        label=dept,
        s=80,
        alpha=0.85,
        edgecolors='black',
        linewidth=0.5
    )

# 3. Add styling, labels, and category legend
plt.title("Age vs. Salary Grouped by Department", fontsize=13, fontweight='bold')
plt.xlabel("Age (Years)", fontsize=11)
plt.ylabel("Salary ($)", fontsize=11)
plt.legend(title="Department", frameon=True, shadow=True)
plt.grid(True, linestyle="--", alpha=0.5)
plt.show()
```

> [!TIP]
> Avoid low-contrast pure colors like pure `"yellow"` on light backgrounds. Use warmer, accessible shades like `"gold"`, `"goldenrod"`, or hex codes (e.g. `"#bcbd22"`).

---

## 9. The Object-Oriented (OO) API & Subplot Dashboards

Matplotlib offers two distinct interfaces:
1. **Pyplot Interface (`plt.*`)**: State-machine based. Functions automatically apply to the "current" active figure and axes. Convenient for quick, single-plot exploration.
2. **Object-Oriented (OO) Interface (`fig, ax`)**: Explicitly instantiates `Figure` and `Axes` objects. Preferred for production pipelines, reusable functions, and multi-panel dashboards.

### Pyplot Procedural Interface vs. Object-Oriented Architecture

| Feature | `pyplot` Procedural API (`plt`) | Object-Oriented API (`fig, ax`) |
| :--- | :--- | :--- |
| **Creation** | `plt.plot(x, y)` | `fig, ax = plt.subplots()` |
| **Philosophy** | Implicit state-machine | Explicit object references |
| **Multiple Plots** | Hard to track active axes | Explicit control via `axs[i]` |
| **Setting Titles** | `plt.title("Text")` | `ax.set_title("Text")` |
| **Setting Labels** | `plt.xlabel("X")`, `plt.ylabel("Y")` | `ax.set_xlabel("X")`, `ax.set_ylabel("Y")` |
| **Setting Limits** | `plt.xlim(0, 10)` | `ax.set_xlim(0, 10)` |
| **Best Used For** | Quick interactive exploration | Multi-panel figures, reports, production code |

### Creating Multi-Panel Grids with `plt.subplots()`

Use `plt.subplots(nrows, ncols, figsize=(width, height))` to build a multi-chart dashboard in a single canvas:

```python
# Create 1 row containing 3 subplots side-by-side
fig, axs = plt.subplots(1, 3, figsize=(16, 5))

# Subplot 0: Sorted Salary Trend (Line Plot)
sort_age = df.sort_values("Age")
axs[0].plot(sort_age["Age"], sort_age["Salary"], color="#1f77b4", marker="o", linewidth=2)
axs[0].set_title("Salary Trend by Age")
axs[0].set_xlabel("Age")
axs[0].set_ylabel("Salary ($)")
axs[0].grid(True, linestyle="--", alpha=0.5)

# Subplot 1: Total Salary Spend by Department (Bar Chart)
salary_by_dept = df.groupby("Dept")['Salary'].sum()
axs[1].bar(salary_by_dept.index, salary_by_dept.values, color="#ff7f0e", edgecolor="black")
axs[1].set_title("Total Budget by Department")
axs[1].set_xlabel("Department")
axs[1].set_ylabel("Total Salary ($)")
axs[1].tick_params(axis='x', rotation=45)

# Subplot 2: Multi-Category Salary Distribution (Box Plot)
depts = df["Dept"].unique()
grouped_salaries = [df[df["Dept"] == d]["Salary"] for d in depts]
axs[2].boxplot(grouped_salaries, tick_labels=depts)
axs[2].set_title("Salary Distribution by Department")
axs[2].set_xlabel("Department")
axs[2].set_ylabel("Salary ($)")
axs[2].tick_params(axis='x', rotation=45)

# Prevent title/label overlaps across panels
plt.tight_layout()
plt.show()
```

### Indexing Subplot Arrays (1D & 2D Grids)

- **1D Array of Subplots (`1 x N` or `N x 1`)**:
  ```python
  fig, axs = plt.subplots(1, 3) # axs is a 1D NumPy array: axs[0], axs[1], axs[2]
  ```
- **2D Grid of Subplots (`M x N`)**:
  ```python
  fig, axs = plt.subplots(2, 2) # axs is a 2D NumPy array: axs[row, col]
  axs[0, 0].plot(...) # Top-left
  axs[0, 1].plot(...) # Top-right
  axs[1, 0].plot(...) # Bottom-left
  axs[1, 1].plot(...) # Bottom-right
  ```
- **Flattening 2D Grids for Easy Loops**:
  ```python
  fig, axs = plt.subplots(2, 3, figsize=(12, 8))
  for ax, col in zip(axs.flatten(), numeric_columns):
      ax.hist(df[col], bins=10)
      ax.set_title(f"Distribution of {col}")
  plt.tight_layout()
  ```

### Object-Oriented Method Mapping Reference

| Action | Pyplot Interface (`plt`) | Object-Oriented Interface (`ax`) |
| :--- | :--- | :--- |
| **Plot Title** | `plt.title("My Title")` | `ax.set_title("My Title")` |
| **X-Axis Label** | `plt.xlabel("X Label")` | `ax.set_xlabel("X Label")` |
| **Y-Axis Label** | `plt.ylabel("Y Label")` | `ax.set_ylabel("Y Label")` |
| **Axis Limits** | `plt.xlim(0, 100)` | `ax.set_xlim(0, 100)` |
| **Ticks Formatting**| `plt.xticks(rotation=45)` | `ax.tick_params(axis='x', rotation=45)` |
| **Grid Lines** | `plt.grid(True)` | `ax.grid(True)` |
| **Legend** | `plt.legend()` | `ax.legend()` |
| **Batch Setting** | *N/A* | `ax.set(title="...", xlabel="...", ylabel="...")` |

### Layout Management with `tight_layout()`

Always call `plt.tight_layout()` before displaying or saving subplots. It automatically calculates padding between subplots, titles, axis labels, and tick marks so that no text is clipped or overlapping.

---

## 10. Essential Styling & Customization Tips

1. **Set Figure Dimensions Early**:
   ```python
   plt.figure(figsize=(10, 6)) # Width 10 inches, Height 6 inches
   ```
2. **Prevent Clipped Labels & Titles**:
   ```python
   plt.tight_layout() # Automatically adjusts margins
   ```
3. **Save High-Resolution Graphics**:
   ```python
   plt.savefig("salary_chart.png", dpi=300, bbox_inches='tight')
   ```
4. **Use Built-in Styles**:
   ```python
   plt.style.use('seaborn-v0_8-whitegrid') # Clean, publication-ready grid style
   ```

---

## 11. Matplotlib Cheatsheet Reference Table

| Chart Type | Pyplot Syntax | OO Subplot Syntax | Best Analytical Use Case |
| :--- | :--- | :--- | :--- |
| **Line Plot** | `plt.plot(x, y)` | `ax.plot(x, y)` | Continuous progression / trend analysis |
| **Histogram** | `plt.hist(x, bins=10)` | `ax.hist(x, bins=10)` | Frequency distribution & skewness |
| **Single Box Plot** | `plt.boxplot(x)` | `ax.boxplot(x)` | 5-number summary & outlier spotting |
| **Grouped Box Plot** | `plt.boxplot(groups, tick_labels=...)` | `ax.boxplot(groups, tick_labels=...)` | Comparing spreads across discrete categories |
| **Bar Chart** | `plt.bar(cats, vals)` | `ax.bar(cats, vals)` | Discrete category counts & magnitude comparison |
| **Aggregated Bar** | `plt.bar(grp.index, grp.values)` | `ax.bar(grp.index, grp.values)` | Group-by sums, averages, and KPIs |
| **Pie Chart** | `plt.pie(vals, labels=..., autopct='%1.1f%%')` | `ax.pie(vals, labels=..., autopct='%1.1f%%')` | Simple part-to-whole percentage shares (<5 categories) |
| **Bivariate Scatter**| `plt.scatter(x, y)` | `ax.scatter(x, y)` | Spotting bivariate correlation & clusters |
| **Multivariate Scatter**| `plt.scatter(x, y, color=c, label=grp)` | `ax.scatter(x, y, color=c, label=grp)` | 2 Numerical variables grouped by categorical hue |
| **Subplot Grid** | `plt.subplot(nrows, ncols, index)` | `fig, axs = plt.subplots(r, c)` | Multi-panel exploratory dashboard |

---

[⬅️ Pandas Guide](./pandas_guide.md) | [Back to README](../README.md) | [Next: Datasets Guide ➡️](./datasets_guide.md)

