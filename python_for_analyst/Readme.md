# Python for Data Analysts

A practical notebook-based introduction to **Python for data analysis**. This folder covers the core NumPy and pandas concepts used to explore, transform, clean, and analyze tabular data.

## Contents

| File | Description |
| --- | --- |
| [`numpy.ipynb`](./numpy.ipynb) | NumPy fundamentals, including arrays, dtypes, reshaping, arithmetic, indexing, slicing, views, copies, aggregation, and logical operations. |
| [`pandas.ipynb`](./pandas.ipynb) | pandas fundamentals, data loading, filtering with logical operators, and data-cleaning techniques. |
| [`data/Housing.csv`](./data/Housing.csv) | Housing data used for exploratory and analytical exercises. |
| [`data/archive/Zomato Restaurant Dataset.csv`](./data/archive/Zomato%20Restaurant%20Dataset.csv) | Restaurant dataset for broader pandas analysis practice. |

## Topics Covered

### NumPy

- Creating and inspecting arrays
- Array data types and type casting
- Shape, reshaping, `ravel`, and `flatten`
- Arithmetic operations and broadcasting
- Universal functions (ufuncs)
- Indexing and slicing
- Views versus copies
- Aggregate functions
- Logical `AND` and `OR` operations

### pandas

- Working with Series and DataFrames
- Reading and inspecting datasets
- Selection and filtering
- Logical operators
- Handling and cleaning data

## Getting Started

### Requirements

- Python 3.x
- Jupyter Notebook or JupyterLab
- NumPy
- pandas

Install the Python packages with pip:

```bash
python -m pip install numpy pandas jupyter
```

### Run the notebooks

From the repository root:

```bash
jupyter notebook python_for_analyst
```

Then open `numpy.ipynb` or `pandas.ipynb`. You can also launch JupyterLab:

```bash
jupyter lab python_for_analyst
```

## Dataset Notes

- **Housing.csv** contains housing attributes such as price, area, number of rooms, amenities, and furnishing status.
- **Zomato Restaurant Dataset.csv** contains restaurant details, locations, cuisines, costs, ratings, and service information.

Paths in the notebooks are relative to the repository and should be run with the `python_for_analyst` folder as their working directory.

## Learning Path

1. Start with `numpy.ipynb` to understand arrays and vectorized operations.
2. Continue with `pandas.ipynb` to apply those concepts to tables and real datasets.
3. Complete exercises using the CSV files in the `data` directory.
4. Try modifying queries and calculations to explore the datasets further.
