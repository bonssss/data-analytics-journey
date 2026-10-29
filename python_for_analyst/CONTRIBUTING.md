# Contributing to Data Analytics Journey 🚀

First off, thank you for considering contributing! This repository is designed as an open-source learning hub for aspiring and practicing data analysts. Whether you are fixing a typo, adding explanatory notes, sharing a new analysis technique, or contributing a practice problem, your contributions are welcome.

---

## Ways You Can Contribute

1. **Improve Documentation & Explanations**:
   - Fix typos, clarify notebook comments, or improve markdown explanations.
   - Add visual diagrams or conceptual analogies to the reading guides in `docs/`.

2. **Add Practice Exercises & Solutions**:
   - Propose real-world analysis questions for the existing datasets (`Housing.csv` and `Zomato Restaurant Dataset.csv`).
   - Create mini-challenges testing specific skills (e.g., advanced filtering, pivot tables, multi-indexing).

3. **Notebook Enhancements**:
   - Add new exploratory data analysis (EDA) techniques.
   - Demonstrate advanced visualization techniques (e.g., heatmaps, subplots, custom color palettes).

4. **Add New Modules**:
   - Add future topic notebooks such as SQL with Python (`sqlite3`, `SQLAlchemy`), Seaborn, or Statistical Hypothesis Testing.

---

## How to Get Started

### 1. Fork & Clone the Repository
```bash
git clone https://github.com/bonssss/data-analytics-journey.git
cd data-analytics-journey/python_for_analyst
```

### 2. Set Up a Virtual Environment
```bash
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Create a Branch
Create a descriptive branch for your work:
```bash
git checkout -b feature/add-housing-eda-exercises
```

### 4. Make Your Changes
- Ensure notebook cell outputs are clean and clear.
- Avoid committing hardcoded absolute paths (e.g. `/home/username/...`); always use paths relative to the project root.
- Document any new code with helpful markdown explanations so learners can understand the *why*, not just the *how*.

### 5. Clear Temporary Outputs Before Committing (Optional)
If your contribution contains large or repetitive outputs, clear unnecessary cell outputs or restart the kernel and run all cells once cleanly:
- Kernel -> Restart & Run All

### 6. Commit & Submit a Pull Request
```bash
git add .
git commit -m "Add exploratory analysis questions to Housing dataset"
git push origin feature/add-housing-eda-exercises
```
Then open a Pull Request on GitHub with a brief description of what was changed and why!

---

## Community Guidelines

- Be supportive, encouraging, and respectful of learners of all skill levels.
- For questions, open a GitHub Discussion or Issue.
- Adhere to our [Code of Conduct](./CODE_OF_CONDUCT.md).
