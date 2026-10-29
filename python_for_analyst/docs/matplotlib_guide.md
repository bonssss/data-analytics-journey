# 📊 Matplotlib Complete Guide & Visualization Handbook

Data visualization is one of the most critical skills for a data analyst. It allows you to explore raw data, uncover hidden anomalies, communicate findings clearly to non-technical stakeholders, and validate modeling assumptions.

---

## 📑 Table of Contents
- [1. Anatomy of a Matplotlib Figure](#1-anatomy-of-a-matplotlib-figure)
- [2. The Chart Selection Framework](#2-the-chart-selection-framework)
- [3. Line Plots: Trends & Formatting](#3-line-plots-trends--formatting)
- [4. Histograms: Frequency Distributions](#4-histograms-frequency-distributions)
- [5. Box Plots: Outlier Detection & 5-Number Summaries](#5-box-plots-outlier-detection--5-number-summaries)
- [6. Categorical Visualization: Bar Charts vs. Pie Charts](#6-categorical-visualization-bar-charts-vs-pie-charts)
- [7. Scatter Plots: Bivariate Analysis & Correlation](#7-scatter-plots-bivariate-analysis--correlation)
- [8. Essential Styling & Customization Tips](#8-essential-styling--customization-tips)
- [9. Matplotlib Cheatsheet Reference Table](#9-matplotlib-cheatsheet-reference-table)

---

## 1. Anatomy of a Matplotlib Figure

In Matplotlib, a plot consists of two foundational objects:
1. **`Figure`**: The top-level canvas / container holding all plot elements.
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

| Analytical Goal | Recommended Chart | Matplotlib Function | Example Question |
| :--- | :--- | :--- | :--- |
| **Trend / Progression** | Line Plot | `plt.plot(x, y)` | How does average home price change over years? |
| **Distribution / Spread** | Histogram | `plt.hist(data, bins)` | How are restaurant food prices distributed? |
| **Outlier Detection** | Box Plot | `plt.boxplot(data)` | Are there extreme executive salaries skewing the mean? |
| **Category Comparison** | Bar Chart | `plt.bar(labels, values)` | Which department has the largest headcount? |
| **Part-to-Whole Ratio** | Pie Chart / Donut | `plt.pie(values, labels)` | What percentage of houses have air conditioning? |
| **Relationship / Correlation** | Scatter Plot | `plt.scatter(x, y)` | Does square footage correlate with home price? |

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
> **Avoid Zigzag Crisscrosses**: When plotting a line graph against a continuous column (like `Age`), always sort the DataFrame by that column first (`df.sort_values('Age')`), otherwise the lines will zigzag backward across the plot!

---

## 4. Histograms: Frequency Distributions

Histograms divide continuous numerical data into discrete bins and count how many observations fall into each bin:

```python
# df['Salary'] distribution with 5 bins
plt.hist(df['Salary'], bins=5, color='teal', edgecolor='black')
plt.title("Salary Frequency Distribution")
plt.xlabel("Salary Range")
plt.ylabel("Frequency (Count)")
plt.show()
```

- **Right-skewed (positive)**: Tail extends toward higher values (typical for income, house prices).
- **Left-skewed (negative)**: Tail extends toward lower values (typical for test scores).
- **Symmetric / Bell-shaped**: Data is evenly distributed around the mean.

---

## 5. Box Plots: Outlier Detection & 5-Number Summaries

A Box Plot displays the **5-Number Summary**:
1. **Minimum** ($Q1 - 1.5 \times IQR$)
2. **First Quartile (Q1 / 25th percentile)**
3. **Median (Q2 / 50th percentile)**
4. **Third Quartile (Q3 / 75th percentile)**
5. **Maximum** ($Q3 + 1.5 \times IQR$)

Any points outside the whiskers are flagged as **outliers** (circles/dots).

```python
plt.boxplot(df['Salary'])
plt.title("Salary Box Plot - Detecting Outliers")
plt.ylabel("Salary ($)")
plt.show()
```

### Handling Outliers:
```python
# 1. Identify outlier record
outlier_index = df[df['Salary'] > 20000].index

# 2. Drop the anomaly to restore scale
df.drop(index=outlier_index, inplace=True)
```

---

## 6. Categorical Visualization: Bar Charts vs. Pie Charts

### Bar Charts (Recommended)
Human eyes are much better at judging rectangular lengths along a common baseline than estimating angles and slices of a circle:

```python
counts = df['Dept'].value_counts()

plt.bar(counts.index, counts.values, color='coral', edgecolor='black')
plt.title("Employee Count by Department")
plt.xlabel("Department")
plt.ylabel("Number of Employees")
plt.xticks(rotation=45) # Rotate category labels for readability
plt.show()
```

### Pie Charts (Use Sparingly)
Only use pie charts when you have 5 or fewer categories and the goal is strictly to show proportions of 100%:

```python
plt.pie(counts, labels=counts.index, autopct="%1.1f%%", startangle=90)
plt.title("Department Distribution")
plt.show()
```

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

## 8. Essential Styling & Customization Tips

1. **Set Figure Dimensions Early**:
   ```python
   plt.figure(figsize=(10, 6)) # Width 10 inches, Height 6 inches
   ```
2. **Prevent Clipped Labels**:
   ```python
   plt.tight_layout() # Automatically adjusts margins
   ```
3. **Save High-Resolution Graphics**:
   ```python
   plt.savefig("salary_chart.png", dpi=300, bbox_inches='tight')
   ```

---

## 9. Matplotlib Cheatsheet Reference Table

| Chart Type | Syntax | Best Used For |
| :--- | :--- | :--- |
| **Line Plot** | `plt.plot(x, y, marker='o')` | Continuous progression / trend analysis |
| **Histogram** | `plt.hist(x, bins=10)` | Frequency distribution & skewness |
| **Box Plot** | `plt.boxplot(x)` | 5-number summary & outlier spotting |
| **Bar Chart** | `plt.bar(categories, values)` | Discrete category counts & magnitude comparison |
| **Pie Chart** | `plt.pie(values, labels=..., autopct='%1.1f%%')` | Simple part-to-whole percentage shares |
| **Scatter Plot** | `plt.scatter(x, y, color=...)` | Spotting bivariate correlation & clusters |

---

[⬅️ Pandas Guide](./pandas_guide.md) | [Back to README](../README.md) | [Next: Datasets Guide ➡️](./datasets_guide.md)
