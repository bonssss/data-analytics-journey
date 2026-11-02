# 🌊 Seaborn Complete Guide & Statistical Visualization Handbook

**Seaborn** is a high-level Python data visualization library built on top of **Matplotlib** and closely integrated with **pandas** data structures. It is specifically designed to make exploratory data analysis (EDA) and statistical visualization intuitive, informative, and visually stunning.

---

## 📑 Table of Contents
- [1. Why Seaborn? (Seaborn vs. Matplotlib)](#1-why-seaborn-seaborn-vs-matplotlib)
- [2. The Seaborn Architecture: Figure-Level vs. Axes-Level Functions](#2-the-seaborn-architecture-figure-level-vs-axes-level-functions)
- [3. Controlling Aesthetics, Styles & Contexts](#3-controlling-aesthetics-styles--contexts)
- [4. Color Palettes & Color Theory for Analysts](#4-color-palettes--color-theory-for-analysts)
- [5. Relational Plots (Scatter & Line)](#5-relational-plots-scatter--line)
- [6. Distribution Plots (Histograms, KDE & Joint Plots)](#6-distribution-plots-histograms-kde--joint-plots)
- [7. Categorical Plots (Strip, Swarm, Box, Violin, Bar, Count)](#7-categorical-plots-strip-swarm-box-violin-bar-count)
- [8. Matrix Plots & Correlation Heatmaps](#8-matrix-plots--correlation-heatmaps)
- [9. Multi-Plot Grids: Pairplot & FacetGrid](#9-multi-plot-grids-pairplot--facetgrid)
- [10. Statistical Modeling & Regression Plots](#10-statistical-modeling--regression-plots)
- [11. Seaborn vs. Matplotlib Comparison Table](#11-seaborn-vs-matplotlib-comparison-table)
- [12. Seaborn Cheatsheet Reference Table](#12-seaborn-cheatsheet-reference-table)

---

## 1. Why Seaborn? (Seaborn vs. Matplotlib)

While Matplotlib gives you granular control over every line, tick, and label, it often requires 10 to 20 lines of code just to format a basic grouped statistical plot.

Seaborn was created to solve this by providing:
1. **Direct DataFrame Integration**: Reference column names directly as strings (e.g., `x="age"`, `y="salary"`, `hue="department"`).
2. **Automated Statistical Aggregation**: Computes means, medians, error bars, and 95% confidence intervals automatically.
3. **Multi-Attribute Visual Mapping**: Maps secondary variables to color (`hue`), marker shapes (`style`), and sizes (`size`) with a single parameter.
4. **Publication-Ready Styling**: Clean default themes, grid lines, and harmonious color palettes that look polished without custom CSS tweaking.

```python
# In Pure Matplotlib (requires manual grouping, colors, and loops):
for dept, group in df.groupby('Department'):
    plt.scatter(group['Age'], group['Salary'], label=dept)
plt.legend()

# In Seaborn (one clean, expressive line):
sns.scatterplot(data=df, x='Age', y='Salary', hue='Department')
```

---

## 2. The Seaborn Architecture: Figure-Level vs. Axes-Level Functions

Understanding this distinction is the single most important conceptual milestone when mastering Seaborn.

```
                    ┌─────────────────────────┐
                    │      Figure-Level       │
                    │   (Controls figure/grid)│
                    └────────────┬────────────┘
         ┌───────────────────────┼───────────────────────┐
         ▼                       ▼                       ▼
      relplot()               displot()               catplot()
   (Relational)            (Distributions)          (Categorical)
   ┌─────┴─────┐           ┌─────┴─────┐           ┌─────┴─────┐
   ▼           ▼           ▼           ▼           ▼           ▼
scatterplot lineplot    histplot    kdeplot     boxplot    barplot
   └─────┬─────┘           └─────┬─────┘     stripplot  violinplot
         └───────────────────────┼──────────────countplot  ...
                                 ▼
                    ┌─────────────────────────┐
                    │       Axes-Level        │
                    │  (Plots on single Axes) │
                    └─────────────────────────┘
```

### 1. Axes-Level Functions
- Examples: `sns.scatterplot`, `sns.histplot`, `sns.boxplot`, `sns.barplot`, `sns.heatmap`.
- Draw onto an existing Matplotlib `Axes` object.
- Fully compatible with `plt.subplots()`:
  ```python
  fig, axes = plt.subplots(1, 2, figsize=(12, 5))
  sns.boxplot(data=df, x='species', y='body_mass_g', ax=axes[0])
  sns.histplot(data=df, x='body_mass_g', ax=axes[1])
  ```

### 2. Figure-Level Functions
- Examples: `sns.relplot`, `sns.displot`, `sns.catplot`, `sns.lmplot`, `sns.pairplot`, `sns.jointplot`.
- Control the entire figure layout and return a `FacetGrid` or `PairGrid` object.
- Use `col="..."` and `row="..."` parameters to generate multi-panel subplots without manual loops:
  ```python
  # Creates a 3-column grid of subplots automatically:
  sns.relplot(data=df, x='flipper_length_mm', y='body_mass_g', hue='species', col='island')
  ```

---

## 3. Controlling Aesthetics, Styles & Contexts

Seaborn separates aesthetics into **styles** (the look of the plot) and **contexts** (the scale of font and line elements).

### Styles (`sns.set_style`)
Seaborn provides 5 built-in themes:
- `darkgrid` (Default): Gray background with white grid lines.
- `whitegrid`: White background with subtle gray grid lines (ideal for tables & reports).
- `dark`: Solid gray background with no grid lines.
- `white`: Clean white background without grid lines.
- `ticks`: Minimalist white background with axis tick marks.

```python
sns.set_style("whitegrid")
```

### Removing Spines (`sns.despine`)
Removes top and right border spines to reduce visual clutter:
```python
sns.scatterplot(data=penguins, x="flipper_length_mm", y="body_mass_g")
sns.despine(top=True, right=True)
```

### Scaling Contexts (`sns.set_context`)
Adjusts font sizes, marker sizes, and line widths for your presentation medium:
- `paper`: Smallest scale, optimized for academic papers.
- `notebook` (Default): Optimized for viewing on standard computer screens.
- `talk`: Moderately enlarged for slide decks.
- `poster`: Maximum scale, optimized for conference poster presentations.

```python
sns.set_context("talk", font_scale=1.1)
```

---

## 4. Color Palettes & Color Theory for Analysts

Selecting the correct color palette is essential for honest, accessible data communication.

```
1. Qualitative (Unordered categories: IT, HR, Marketing)
   [ Teal ] [ Orange ] [ Purple ] [ Green ]

2. Sequential (Ordered numeric values: 0 to 100)
   [ Light Blue ] ----------> [ Deep Navy ]

3. Diverging (Positive/negative deviations around a zero midpoint)
   [ Blue (-) ] <--------- [ White (0) ] ---------> [ Red (+) ]
```

### 1. Qualitative Palettes (Categorical Data)
For distinct, unordered groups (e.g., species, countries, payment methods):
- Recommended: `'Dark2'`, `'Set2'`, `'tab10'`, `'pastel'`
```python
sns.color_palette("Dark2")
```

### 2. Sequential Palettes (Ordered / Numeric Intensity)
For continuous values that progress from low to high:
- Recommended: `'viridis'`, `'Blues'`, `'mako'`, `'rocket'`
```python
sns.color_palette("Blues", as_cmap=True)
```

### 3. Diverging Palettes (Bipolar Variation)
For values with a meaningful center point (e.g., net profit/loss, correlation from -1 to +1, temperature anomalies):
- Recommended: `'coolwarm'`, `'vlag'`, `'icefire'`
```python
sns.color_palette("coolwarm", as_cmap=True)
```

---

## 5. Relational Plots (Scatter & Line)

### Scatter Plots (`sns.scatterplot`)
Used for analyzing relationships between continuous variables.

```python
plt.figure(figsize=(8, 5))
sns.scatterplot(
    data=penguins,
    x="flipper_length_mm",
    y="body_mass_g",
    hue="species",       # Colors points by species
    style="sex",         # Changes marker shapes by sex
    size="bill_depth_mm",# Scales marker area by bill depth
    sizes=(20, 200),
    alpha=0.7,
    palette="Dark2"
)
plt.title("4-Dimensional Penguin Morphometrics Analysis")
plt.show()
```

### Line Plots with Confidence Intervals (`sns.lineplot`)
Automatically aggregates multiple measurements per X-value, rendering the mean line along with a shaded 95% confidence interval:

```python
# Automatically aggregates multiple values per year and draws a 95% CI band:
sns.lineplot(data=flights, x="year", y="passengers", hue="month")
```

---

## 6. Distribution Plots (Histograms, KDE & Joint Plots)

### Histogram with KDE (`sns.histplot`)
```python
sns.histplot(
    data=penguins,
    x="body_mass_g",
    hue="species",
    kde=True,            # Overlays smooth density curve
    bins=25,
    element="step",      # Stepped bars prevent occlusion
    palette="muted"
)
```

### Kernel Density Estimation (`sns.kdeplot`)
```python
sns.kdeplot(
    data=penguins,
    x="flipper_length_mm",
    hue="species",
    fill=True,           # Fills area under density curve
    common_norm=False,   # Normalizes each species independently
    alpha=0.3
)
```

### Joint Distributions (`sns.jointplot`)
Combines a bivariate relationship with univariate marginal distributions on the top and right margins:

```python
sns.jointplot(
    data=penguins,
    x="bill_length_mm",
    y="bill_depth_mm",
    hue="species",
    kind="scatter",      # Options: 'scatter', 'kde', 'hist', 'hex', 'reg'
    height=6
)
```

---

## 7. Categorical Plots (Strip, Swarm, Box, Violin, Bar, Count)

When comparing a continuous variable across categorical groups:

| Goal | Recommended Plot | Function | Description |
| :--- | :--- | :--- | :--- |
| **Inspect Raw Observations** | Strip Plot | `sns.stripplot(..., jitter=True)` | Shows all observations; jitter prevents severe overlap. |
| **Exact Density & Count** | Swarm Plot | `sns.swarmplot(...)` | Adjusts points along categorical axis without any overlap. |
| **5-Number Summary & Outliers**| Box Plot | `sns.boxplot(...)` | Displays Q1, Median, Q3, Whiskers, and outliers. |
| **Multi-Modal Distributions** | Violin Plot | `sns.violinplot(..., split=True)` | Combines boxplot with rotated KDE curves. |
| **Compare Statistical Means** | Bar Plot | `sns.barplot(..., errorbar="sd")` | Computes mean and renders error bars. |
| **Category Frequencies** | Count Plot | `sns.countplot(...)` | Displays observation count per category. |

```python
# Example: Split Violin Plot comparing male vs female flipper length across species
plt.figure(figsize=(8, 5))
sns.violinplot(
    data=penguins,
    x="species",
    y="flipper_length_mm",
    hue="sex",
    split=True,         # Male on left half, Female on right half
    inner="quart",      # Shows quartile lines inside violin
    palette="muted"
)
plt.title("Flipper Length Distribution by Species & Sex")
plt.show()
```

> [!TIP]
> **Modern Seaborn Best Practice (v0.13+)**:
> When using `palette` in `barplot` or `countplot` without a distinct second variable, pass `hue=x` and `legend=False` to prevent deprecation warnings:
> ```python
> sns.barplot(data=penguins, x="species", y="body_mass_g", hue="species", legend=False, palette="Blues_d")
> ```

---

## 8. Matrix Plots & Correlation Heatmaps

Correlation heatmaps quantify the strength and direction of linear relationships between numerical features.

```python
# 1. Compute Pearson correlation matrix
corr = df.select_dtypes(include=np.number).corr()

# 2. Render styled heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(
    corr,
    annot=True,          # Displays correlation coefficient numbers
    fmt=".2f",          # 2 decimal places
    cmap="coolwarm",     # Diverging colormap (-1 blue, 0 white, +1 red)
    vmin=-1,
    vmax=1,
    linewidths=1,
    linecolor="white",
    square=True          # Forces square cells
)
plt.title("Feature Correlation Matrix")
plt.show()
```

---

## 9. Multi-Plot Grids: Pairplot & FacetGrid

### Pairplot (`sns.pairplot`)
Generates an $N \times N$ matrix of scatter plots for all numerical features, with univariate distributions along the diagonal:

```python
sns.pairplot(
    penguins,
    hue="species",
    palette="Dark2",
    diag_kind="kde",
    corner=True          # Removes redundant upper triangle
)
```

### FacetGrid / `catplot` Small Multiples
Generates a structured grid of subplots segmented by categorical variables:

```python
sns.catplot(
    data=penguins,
    x="species",
    y="body_mass_g",
    hue="sex",
    col="island",        # Separate subplot for each island
    kind="box",
    height=4,
    aspect=0.9
)
```

---

## 10. Statistical Modeling & Regression Plots

Seaborn includes built-in regression capabilities that fit ordinary least squares (OLS) linear regressions and draw shaded 95% confidence intervals:

### Axes-Level: `sns.regplot`
```python
sns.regplot(
    data=penguins,
    x="flipper_length_mm",
    y="body_mass_g",
    scatter_kws={"alpha": 0.4, "color": "teal"},
    line_kws={"color": "darkred", "linewidth": 2}
)
```

### Figure-Level: `sns.lmplot` (Faceted Regression)
```python
# Fits a separate regression line for each species:
sns.lmplot(
    data=penguins,
    x="flipper_length_mm",
    y="body_mass_g",
    hue="species",
    palette="Set1",
    aspect=1.4,
    height=5
)
```

---

## 11. Seaborn vs. Matplotlib Comparison Table

| Feature | Matplotlib | Seaborn |
| :--- | :--- | :--- |
| **Abstraction Level** | Low-level (imperative) | High-level (declarative) |
| **Data Input** | Python lists, NumPy arrays, Series | Native pandas DataFrames (`data=df, x='col'`) |
| **Statistical Calculations** | Manual calculation required | Automatic (means, standard deviations, CIs, KDE) |
| **Multi-Attribute Mapping** | Manual loops and custom color dictionaries | Native (`hue`, `style`, `size`) |
| **Subplot Grids** | Manual `plt.subplots()` loops | Native (`col`, `row`, `pairplot`, `FacetGrid`) |
| **Customizability** | 100% granular control over every pixel | Fast defaults, fully extensible with Matplotlib |

---

## 12. Seaborn Cheatsheet Reference Table

| Category | Function | Essential Parameters | Use Case |
| :--- | :--- | :--- | :--- |
| **Styling** | `sns.set_theme()` | `style='whitegrid', context='notebook'` | Apply global aesthetic style |
| | `sns.despine()` | `top=True, right=True` | Remove chart border spines |
| **Relational** | `sns.scatterplot()` | `data, x, y, hue, style, size, palette` | Bivariate scatter points with grouping |
| | `sns.lineplot()` | `data, x, y, hue, errorbar` | Trends over time with confidence interval bands |
| | `sns.relplot()` | `data, x, y, hue, col, row, kind` | Faceted relational subplots |
| **Distribution** | `sns.histplot()` | `data, x, hue, kde=True, bins, element` | Binned distribution counts & density |
| | `sns.kdeplot()` | `data, x, hue, fill=True, common_norm` | Continuous probability density curves |
| | `sns.jointplot()` | `data, x, y, hue, kind` | Bivariate + univariate marginals |
| | `sns.displot()` | `data, x, hue, col, kind` | Faceted distribution subplots |
| **Categorical** | `sns.stripplot()` | `data, x, y, hue, jitter=True` | Individual observations along category axis |
| | `sns.swarmplot()` | `data, x, y, hue` | Non-overlapping points showing exact density |
| | `sns.boxplot()` | `data, x, y, hue, palette` | 5-number summary & statistical outliers |
| | `sns.violinplot()`| `data, x, y, hue, split=True, inner` | Hybrid box plot + KDE distribution |
| | `sns.barplot()` | `data, x, y, hue, estimator, errorbar` | Group averages with uncertainty error bars |
| | `sns.countplot()` | `data, x, hue, palette` | Observation frequency counts |
| | `sns.catplot()` | `data, x, y, hue, col, kind` | Faceted categorical subplots |
| **Matrix** | `sns.heatmap()` | `data, annot=True, fmt, cmap, square` | 2D correlation matrix visualization |
| **Multi-Plot** | `sns.pairplot()` | `data, hue, diag_kind, corner=True` | Full pairwise exploratory data analysis (EDA) |
| **Regression** | `sns.regplot()` | `data, x, y, scatter_kws, line_kws` | Linear fit with confidence intervals |
| | `sns.lmplot()` | `data, x, y, hue, col, aspect, height` | Grouped & faceted regression modeling |

---

[⬅️ Matplotlib Guide](./matplotlib_guide.md) | [Back to README](../README.md) | [Next: Datasets Guide ➡️](./datasets_guide.md)
