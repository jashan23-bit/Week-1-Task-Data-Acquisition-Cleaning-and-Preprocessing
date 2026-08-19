[README.md](https://github.com/user-attachments/files/31210923/README.md)
# Titanic Dataset — Data Exploration & Cleaning

## Project Overview

This project explores and cleans the Titanic passenger dataset using Python and pandas.

The workflow is divided into two Jupyter notebooks:

1. **`01_exploration_jupyter.ipynb`** — Exploratory data analysis and data-quality checks.
2. **`03_cleaning_jupyter.ipynb`** — Data cleaning, feature engineering, consistency checks, and creation of modeling-ready datasets.

---

## Dataset

The notebooks expect the raw dataset to be available as:

```text
titanic_raw.csv
```

The dataset contains Titanic passenger information such as:

- Passenger ID
- Survival status
- Passenger class
- Name
- Sex
- Age
- Number of siblings/spouses aboard
- Number of parents/children aboard
- Ticket
- Fare
- Cabin
- Port of embarkation

---

## Notebooks

### 1. `01_exploration_jupyter.ipynb`

This notebook performs initial data exploration, including:

- Loading the Titanic dataset
- Checking the dataset shape
- Inspecting data types
- Viewing the first 10 records
- Generating numerical descriptive statistics
- Generating categorical descriptive statistics
- Identifying missing values and their percentages
- Checking duplicate rows
- Checking duplicate `PassengerId` values
- Inspecting unique values for key categorical variables
- Checking the range of `Age` and `Fare`
- Identifying zero-fare records
- Inspecting fractional age values

### 2. `03_cleaning_jupyter.ipynb`

This notebook prepares the dataset for downstream analysis/modeling.

#### Data type conversion

The following columns are converted to categorical data types:

- `Survived`
- `Pclass`
- `Sex`

#### Missing `Embarked` values

Missing `Embarked` values are filled using the mode of the column.

#### Missing `Age` values

Missing ages are imputed using the median age grouped by:

- `Pclass`
- `Sex`
- `Title`

A fallback using `Pclass` and `Sex` is applied if any missing ages remain.

#### Title extraction

Titles are extracted from the `Name` column and grouped where appropriate. Examples include:

- `Mlle` → `Miss`
- `Ms` → `Miss`
- `Mme` → `Mrs`
- Several uncommon titles → `Rare`

#### Age features

An `AgeGroup` feature is created with these categories:

- Child
- Teen
- Young Adult
- Adult
- Senior

#### Cabin features

Because the `Cabin` column contains substantial missing data, two derived features are created:

- `HasCabin` — binary indicator showing whether cabin information exists
- `Deck` — first cabin letter, with `Unknown` used when cabin information is missing

#### Fare features

Zero-fare passengers are retained because the notebook treats these records as legitimate rather than data-entry errors.

The following features are created:

- `FareIsZero`
- `FareCapped`
- `FareLog`

High-fare observations are retained as valid observations. An IQR-based upper bound is used for the capped feature, while `FareLog` applies a `log1p` transformation.

#### Family features

The following features are derived:

- `FamilySize = SibSp + Parch + 1`
- `IsAlone` — indicates whether the passenger was traveling alone

#### Consistency checks

The notebook checks:

- Large family sizes
- Title/sex mismatches

#### Modeling-ready dataset

The raw `Cabin`, `Ticket`, and `Name` columns are removed from the modeling-ready version because useful information from `Cabin` has already been engineered and `Ticket`/`Name` are not retained in the final modeling dataset.

---

## Output Files

The cleaning notebook produces two CSV files:

```text
titanic_cleaned_full.csv
titanic_model_ready.csv
```

### `titanic_cleaned_full.csv`

Contains the cleaned dataset with the original columns retained for reporting and traceability, along with engineered features.

### `titanic_model_ready.csv`

Contains the cleaned, feature-engineered dataset with these raw columns removed:

- `Cabin`
- `Ticket`
- `Name`

---

## Project Structure

A recommended project structure is:

```text
titanic-project/
│
├── titanic_raw.csv
├── 01_exploration_jupyter.ipynb
├── 03_cleaning_jupyter.ipynb
├── titanic_cleaned_full.csv
├── titanic_model_ready.csv
└── README.md
```

---

## Requirements

The notebooks use:

- Python 3
- pandas
- NumPy
- Jupyter Notebook or JupyterLab

Install the required Python packages with:

```bash
pip install pandas numpy jupyter
```

---

## How to Run

1. Place `titanic_raw.csv` in the same directory as the notebooks.
2. Open Jupyter Notebook or JupyterLab.
3. Run `01_exploration_jupyter.ipynb` to inspect the raw dataset.
4. Run `03_cleaning_jupyter.ipynb` to clean and transform the data.
5. The cleaned CSV files will be generated in the same working directory.

---

## Data Processing Flow

```text
titanic_raw.csv
       │
       ▼
01_exploration_jupyter.ipynb
       │
       │  Explore structure, missing values,
       │  duplicates, ranges, and categories
       ▼
03_cleaning_jupyter.ipynb
       │
       │  Clean data + engineer features
       ▼
 ┌───────────────────────────────┐
 │ titanic_cleaned_full.csv      │
 │ titanic_model_ready.csv       │
 └───────────────────────────────┘
```

## Notes

The notebooks retain valid extreme observations rather than automatically deleting them. In particular, zero-fare and high-fare records are investigated and transformed where useful instead of being removed.

This project currently focuses on **data exploration and cleaning**. A machine-learning modeling stage is not included in the two notebooks described above.
