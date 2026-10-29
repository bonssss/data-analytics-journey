# 📂 Datasets Guide & Practice Challenges

This repository includes two real-world datasets in the `data/` directory to help you practice data wrangling, cleaning, exploratory data analysis (EDA), and data visualization.

---

## 📑 Table of Contents
- [1. Housing Dataset (`data/Housing.csv`)](#1-housing-dataset-datahousingcsv)
  - [Schema & Data Dictionary](#housing-schema--data-dictionary)
  - [Practice Challenges (Exercises 1 - 5)](#housing-practice-challenges)
- [2. Zomato Restaurant Dataset (`data/archive/Zomato Restaurant Dataset.csv`)](#2-zomato-restaurant-dataset)
  - [Schema & Data Dictionary](#zomato-schema--data-dictionary)
  - [Practice Challenges (Exercises 6 - 10)](#zomato-practice-challenges)
- [3. Code Snippets to Load the Data](#3-code-snippets-to-load-the-data)

---

## 1. Housing Dataset (`data/Housing.csv`)

A tabular dataset of real estate properties, listing physical attributes, amenities, and sale prices.

### Housing Schema & Data Dictionary

| Column Name | Data Type | Description | Values / Examples |
| :--- | :--- | :--- | :--- |
| `price` | Integer | Sale price of the house | e.g., `13300000`, `12250000` |
| `area` | Integer | Total area in square feet | e.g., `7420`, `8960` |
| `bedrooms` | Integer | Number of bedrooms | `1` to `6` |
| `bathrooms` | Integer | Number of bathrooms | `1` to `4` |
| `stories` | Integer | Number of stories/floors | `1` to `4` |
| `mainroad` | Categorical (String) | Connected to main road? | `'yes'`, `'no'` |
| `guestroom` | Categorical (String) | Presence of guest room? | `'yes'`, `'no'` |
| `basement` | Categorical (String) | Presence of basement? | `'yes'`, `'no'` |
| `hotwaterheating`| Categorical (String) | Hot water heating system? | `'yes'`, `'no'` |
| `airconditioning`| Categorical (String) | Central air conditioning? | `'yes'`, `'no'` |
| `parking` | Integer | Number of parking spots | `0`, `1`, `2`, `3` |
| `prefarea` | Categorical (String) | Located in preferred area?| `'yes'`, `'no'` |
| `furnishingstatus`| Categorical (String) | Furnishing condition | `'furnished'`, `'semi-furnished'`, `'unfurnished'` |

---

### Housing Practice Challenges

#### Challenge 1: Distribution & Outliers in Price
- **Task**: Plot a histogram and a box plot for `price`.
- **Question**: Is the house price distribution normal or skewed? Are there upper outliers?
- *Hint*: Use `plt.hist(df['price'], bins=20)` and `plt.boxplot(df['price'])`.

#### Challenge 2: Area vs. Price Relationship
- **Task**: Create a scatter plot of `area` (X-axis) against `price` (Y-axis).
- **Question**: Is there a strong positive correlation between square footage and house price?
- *Hint*: Use `plt.scatter(df['area'], df['price'], alpha=0.5)`.

#### Challenge 3: Impact of Air Conditioning on Price
- **Task**: Calculate the average and median `price` for houses with `airconditioning == 'yes'` vs. `airconditioning == 'no'`.
- **Question**: What is the price premium for houses with air conditioning?
- *Hint*: Use `df.groupby('airconditioning')['price'].agg(['mean', 'median'])`.

#### Challenge 4: Furnishing Status Breakdown
- **Task**: Count how many houses fall into each `furnishingstatus` category and plot a bar chart.
- *Hint*: Use `counts = df['furnishingstatus'].value_counts()` and `plt.bar(counts.index, counts.values)`.

#### Challenge 5: Multi-Condition Filtering (Luxury Homes)
- **Task**: Filter the DataFrame for houses that have at least 4 bedrooms, 2 bathrooms, parking for at least 2 cars, and are in a preferred area (`prefarea == 'yes'`).
- *Hint*: Combine conditions with `&` inside `df[(df['bedrooms'] >= 4) & ...]`.

---

## 2. Zomato Restaurant Dataset

Located at: `data/archive/Zomato Restaurant Dataset.csv`. Over 9,500 restaurant listings across worldwide cities with ratings, cost for two, and delivery attributes.

### Zomato Schema & Data Dictionary

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `Restaurant ID` | Integer | Unique identifier for the restaurant |
| `Restaurant Name`| String | Name of the restaurant |
| `Country Code` | Integer | Country identifier code |
| `City` | String | City where restaurant is located |
| `Address` | String | Street address |
| `Locality` | String | Locality or neighborhood |
| `Longitude` | Float | Geographic longitude coordinate |
| `Latitude` | Float | Geographic latitude coordinate |
| `Cuisines` | String | Comma-separated list of cuisine types |
| `Average Cost for two`| Integer | Estimated cost for a meal for two people |
| `Currency` | String | Local transaction currency |
| `Has Table booking`| String (`Yes`/`No`) | Whether table reservation is supported |
| `Has Online delivery`| String (`Yes`/`No`)| Whether online ordering/delivery is active |
| `Is delivering now`| String (`Yes`/`No`)| Live delivery status |
| `Price range` | Integer (1 to 4) | Relative price tier indicator |
| `Aggregate rating`| Float | Overall rating score (0.0 to 5.0) |
| `Rating color` | String | Color badge corresponding to rating |
| `Rating text` | String | Qualitative score (`Excellent`, `Good`, `Average`, `Poor`) |
| `Votes` | Integer | Total count of user reviews and votes |

---

### Zomato Practice Challenges

#### Challenge 6: Top Cities by Restaurant Count
- **Task**: Find the top 10 cities with the most restaurants listed in the dataset and display them with a horizontal bar chart (`plt.barh`).
- *Hint*: Use `df['City'].value_counts().head(10)`.

#### Challenge 7: Does Online Delivery Lead to Higher Ratings?
- **Task**: Compare the mean and median `Aggregate rating` between restaurants that offer online delivery (`Has Online delivery == 'Yes'`) vs. those that do not.
- *Hint*: Filter out unrated restaurants (`Aggregate rating > 0`) first!

#### Challenge 8: Identifying Common Cuisines
- **Task**: Inspect the `Cuisines` column. Count how many missing/NaN entries exist and drop or impute them.
- *Hint*: Use `df['Cuisines'].isnull().sum()` and `df.dropna(subset=['Cuisines'])`.

#### Challenge 9: Price Range vs. Cost for Two
- **Task**: Group by `Price range` (1 to 4) and calculate the average `Average Cost for two`. Plot the result.
- *Hint*: Filter to a specific single currency (e.g., Indian Rupee `Rs.` or US Dollar `$`) to ensure fair comparisons!

#### Challenge 10: High-Vote Rated Gems
- **Task**: Find all restaurants with an `Aggregate rating >= 4.5` and at least 500 `Votes`. Display their name, city, and cuisines.
- *Hint*: `df[(df['Aggregate rating'] >= 4.5) & (df['Votes'] >= 500)][['Restaurant Name', 'City', 'Cuisines', 'Aggregate rating', 'Votes']]`.

---

## 3. Code Snippets to Load the Data

Use this snippet in your Jupyter notebooks or scripts to load both datasets reliably:

```python
import os
import pandas as pd

# 1. Load Housing dataset
housing_file = "data/Housing.csv" if os.path.exists("data/Housing.csv") else "Housing.csv"
housing_df = pd.read_csv(housing_file)
print(f"Housing loaded: {housing_df.shape[0]} rows, {housing_df.shape[1]} columns")

# 2. Load Zomato dataset
zomato_file = (
    "data/archive/Zomato Restaurant Dataset.csv"
    if os.path.exists("data/archive/Zomato Restaurant Dataset.csv")
    else ("archive/Zomato Restaurant Dataset.csv"
          if os.path.exists("archive/Zomato Restaurant Dataset.csv")
          else "Zomato Restaurant Dataset.csv")
)
zomato_df = pd.read_csv(zomato_file, encoding="utf-8")
print(f"Zomato loaded: {zomato_df.shape[0]} rows, {zomato_df.shape[1]} columns")
```

---

[⬅️ Matplotlib Guide](./matplotlib_guide.md) | [Back to README](../README.md)
