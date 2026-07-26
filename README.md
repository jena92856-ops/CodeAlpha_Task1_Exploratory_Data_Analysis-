# Retail Sales Dataset — Exploratory Data Analysis (EDA)

A Python-based exploratory data analysis of a retail sales transactions dataset. The project inspects and cleans the raw data, then analyzes customer demographics, revenue by gender and product category, top transactions, and relationships between quantity, price, and outliers.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Usage](#-usage)
- [Analysis Walkthrough](#-analysis-walkthrough)
  - [1. Setup & Imports](#1-setup--imports)
  - [2. Loading the Data](#2-loading-the-data)
  - [3. Initial Inspection](#3-initial-inspection)
  - [4. Data Cleaning](#4-data-cleaning)
  - [5. Datatype Analysis](#5-datatype-analysis)
  - [6. Descriptive Statistics — Age](#6-descriptive-statistics--age)
  - [7. Customer Age Distribution](#7-customer-age-distribution)
  - [8. Gender Distribution](#8-gender-distribution)
  - [9. Sales by Gender](#9-sales-by-gender)
  - [10. Product Category Distribution](#10-product-category-distribution)
  - [11. Revenue by Product Category](#11-revenue-by-product-category)
  - [12. Total Revenue Calculation](#12-total-revenue-calculation)
  - [13. Average Transaction Value](#13-average-transaction-value)
  - [14. Top 10 Highest Value Transactions](#14-top-10-highest-value-transactions)
  - [15. Quantity vs Total Amount Analysis](#15-quantity-vs-total-amount-analysis)
  - [16. Outlier Detection](#16-outlier-detection)
  - [17. Correlation Heatmap](#17-correlation-heatmap)
- [Key Findings](#-key-findings)
- [Graphs Generated](#-graphs-generated)
- [License](#-license)

---

## 📖 Overview

This notebook (`Task_1.ipynb`) performs an end-to-end exploratory data analysis on a retail sales transactions dataset (`retail_sales_dataset.csv`). It covers:

- Data loading and structural inspection (shape, dtypes, summary statistics)
- Checking and removing missing values and duplicate rows
- Analyzing customer demographics (age, gender)
- Revenue breakdowns by gender and product category
- Identifying top-value transactions
- Studying the relationship between quantity purchased and transaction value
- Detecting outliers in transaction amounts
- Examining correlations between numeric fields

---

## 📊 Dataset

The dataset used is `retail_sales_dataset.csv`, containing individual retail transactions with fields such as:

| Column | Description |
|---|---|
| `Transaction ID` | Unique identifier for the transaction |
| `Date` | Date of the transaction |
| `Customer ID` | Unique identifier for the customer |
| `Gender` | Customer gender |
| `Age` | Customer age |
| `Product Category` | Category of the purchased product |
| `Quantity` | Number of units purchased |
| `Price per Unit` | Unit price of the product |
| `Total Amount` | Total transaction value (`Quantity × Price per Unit`) |

> **Note:** The raw CSV is not included in this repository. Place `retail_sales_dataset.csv` in a `/content` (Colab) or local `data/` folder and update the file path in the notebook accordingly before running.

---

## 🛠 Tech Stack

- **Python 3**
- **pandas** — data loading, cleaning, and manipulation
- **numpy** — numerical operations
- **matplotlib** — base plotting
- **seaborn** — statistical visualizations
- Developed and tested in **Google Colab**

---

## 📁 Project Structure

```
.
├── Task_1.ipynb                    # Main analysis notebook
├── data/
│   └── retail_sales_dataset.csv    # Raw dataset (not tracked in repo)
└── README.md                        # Project documentation
```

---

## ⚙️ Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```

2. Create a virtual environment (optional but recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate      # On Windows: venv\Scripts\activate
   ```

3. Install the required dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```

4. Add the dataset (`retail_sales_dataset.csv`) to your working directory or update the file path used in the notebook.

---

## ▶️ Usage

1. Launch Jupyter Notebook or open the file in Google Colab:
   ```bash
   jupyter notebook Task_1.ipynb
   ```

2. Update the dataset path if needed:
   ```python
   file = pd.read_csv('/content/retail_sales_dataset.csv')
   # or, for a local copy:
   # file = pd.read_csv('data/retail_sales_dataset.csv')
   ```

3. Run all cells sequentially (`Runtime > Run all` in Colab, or `Kernel > Restart & Run All` in Jupyter).

---

## 🔍 Analysis Walkthrough

### 1. Setup & Imports

Core libraries are imported and the seaborn plotting style is set for consistent, clean visuals across the notebook.

```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style("whitegrid")
```

### 2. Loading the Data

The dataset is read into a pandas DataFrame, and the first five rows are previewed.

```python
file = pd.read_csv('/content/retail_sales_dataset.csv')
file.head()
```

### 3. Initial Inspection

Basic structural checks are performed: dataset shape, column info/data types, column names, and summary statistics.

```python
print("Dataset Shape :", file.shape)
```

```python
print("Dataset info :", file.info())
```

```python
print("Dataset columns :", file.columns)
```

```python
print("Dataset Describe :", file.describe())
```

### 4. Data Cleaning

Missing values and duplicate rows are checked for, and duplicates are dropped in place.

```python
print("Dataset isnull.sum :", file.isnull().sum())
```

```python
print("Dataset duplicated.sum :", file.duplicated().sum())
```

```python
file.drop_duplicates(inplace=True)
```

### 5. Datatype Analysis

Column data types are reviewed, and object (categorical/text) columns are isolated.

```python
print("Dataset Datatype :", file.dtypes)
```

```python
file.select_dtypes(include='object').columns
```

### 6. Descriptive Statistics — Age

Basic summary statistics (mean, median, min, max) are computed for the `Age` column.

```python
print("Average Age:", file['Age'].mean())
print("Median Age:", file['Age'].median())
print("Minimum Age:", file['Age'].min())
print("Maximum Age:", file['Age'].max())
```

### 7. Customer Age Distribution

A histogram with a KDE overlay visualizes how customer ages are spread across the dataset.

```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(8,5))

sns.histplot(file['Age'], bins=20, kde=True)

plt.title("Customer Age Distribution")
plt.xlabel("Age")
plt.ylabel("Count")

plt.show()
```

📈![image alt](https://github.com/jena92856-ops/CodeAlpha_Task1_Exploratory_Data_Analysis-/blob/ec5f9ebac7eef105ad55329438a4dfaf3d8f4d8c/01_age_distribution.png) 

### 8. Gender Distribution

A count plot shows the number of transactions broken down by customer gender.

```python
plt.figure(figsize=(6,4))

sns.countplot(x='Gender', data=file)

plt.title("Gender Distribution")

plt.show()
```

📈![image alt](

### 9. Sales by Gender

Total revenue (`Total Amount`) is aggregated by gender and compared in a bar chart.

```python
gender_sales = file.groupby('Gender')['Total Amount'].sum()

gender_sales

plt.figure(figsize=(7,5))

gender_sales.plot(kind='bar')

plt.title("Total Sales by Gender")
plt.xlabel("Gender")
plt.ylabel("Revenue")

plt.show()
```

📈 **Graph:** *Total Sales by Gender* — bar chart comparing total revenue generated by each gender.

### 10. Product Category Distribution

A count plot shows how many transactions fall under each product category.

```python
plt.figure(figsize=(8,5))

sns.countplot(
    x='Product Category',
    data=file
)

plt.title("Product Category Distribution")

plt.xticks(rotation=45)

plt.show()
```

📈 **Graph:** *Product Category Distribution* — count plot of transactions per product category.

### 11. Revenue by Product Category

Total revenue is aggregated per product category (sorted ascending) and plotted as a bar chart.

```python
category_revenue = file.groupby(
    'Product Category'
)['Total Amount'].sum().sort_values()

plt.figure(figsize=(10,6))

category_revenue.plot(kind='bar')

plt.title("Revenue by Product Category")

plt.xlabel("Category")
plt.ylabel("Total Revenue")

plt.xticks(rotation=45)

plt.show()
```

📈 **Graph:** *Revenue by Product Category* — bar chart of total revenue generated per category.

### 12. Total Revenue Calculation

The overall revenue generated across all transactions is computed.

```python
total_revenue = file['Total Amount'].sum()

print("Total Revenue Generated:", total_revenue)
```

### 13. Average Transaction Value

The mean value of a single transaction is calculated.

```python
average_transaction = file['Total Amount'].mean()

print(
    "Average Transaction Value:",
    average_transaction
)
```

### 14. Top 10 Highest Value Transactions

The dataset is sorted by `Total Amount` (descending) to find the 10 highest-value transactions, visualized as a bar chart.

```python
top_transactions = file.sort_values(
    by='Total Amount',
    ascending=False
).head(10)

top_transactions

plt.figure(figsize=(10,5))

sns.barplot(
    x='Transaction ID',
    y='Total Amount',
    data=top_transactions
)

plt.title("Top 10 Highest Transactions")

plt.xticks(rotation=45)

plt.show()
```

📈 **Graph:** *Top 10 Highest Transactions* — bar chart of the 10 highest-value transactions by Transaction ID.

### 15. Quantity vs Total Amount Analysis

A scatter plot explores the relationship between the quantity of items purchased and the resulting total transaction amount.

```python
plt.figure(figsize=(8,5))

sns.scatterplot(
    x='Quantity',
    y='Total Amount',
    data=file
)

plt.title("Quantity vs Total Amount")

plt.show()
```

📈 **Graph:** *Quantity vs Total Amount* — scatter plot comparing units purchased to total transaction value.

### 16. Outlier Detection

A box plot is used to visually detect outliers in the `Total Amount` field.

```python
plt.figure(figsize=(8,5))

sns.boxplot(
    y=file['Total Amount']
)

plt.title("Outlier Detection in Total Amount")

plt.show()
```

📈 **Graph:** *Outlier Detection in Total Amount* — box plot highlighting outliers in transaction values.

### 17. Correlation Heatmap

A correlation matrix across all numeric fields is computed and rendered as an annotated heatmap.

```python
plt.figure(figsize=(8,6))

correlation = file.corr(numeric_only=True)

sns.heatmap(
    correlation,
    annot=True,
    cmap='coolwarm'
)

plt.title("Correlation Heatmap")

plt.show()
```

📈 **Graph:** *Correlation Heatmap* — annotated heatmap of pairwise correlations between numeric fields (Age, Quantity, Price per Unit, Total Amount).

---

## 🔑 Key Findings

- Customer ages are distributed across a broad range, giving insight into the core customer base.
- Revenue is compared across gender groups to identify which segment contributes more to total sales.
- Certain product categories generate disproportionately higher revenue than others.
- A handful of transactions account for a large share of total revenue (top 10 highest-value transactions).
- Quantity purchased and total transaction amount show a visible relationship, as expected.
- The box plot on `Total Amount` reveals the presence of outlier transactions worth investigating further.
- The correlation heatmap quantifies how strongly Age, Quantity, Price per Unit, and Total Amount relate to one another.

---

## 📉 Graphs Generated

| # | Chart | Type |
|---|---|---|
| 1 | Customer Age Distribution | Histogram + KDE |
| 2 | Gender Distribution | Count plot |
| 3 | Total Sales by Gender | Bar chart |
| 4 | Product Category Distribution | Count plot |
| 5 | Revenue by Product Category | Bar chart |
| 6 | Top 10 Highest Transactions | Bar chart |
| 7 | Quantity vs Total Amount | Scatter plot |
| 8 | Outlier Detection in Total Amount | Box plot |
| 9 | Correlation Heatmap | Heatmap |

---

## 📄 License

This project is provided for educational purposes. Add a license of your choice (e.g., MIT) if you plan to distribute this repository publicly.
