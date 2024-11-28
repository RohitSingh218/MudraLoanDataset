# Mudra Loan Dataset Analysis

## Objective

The objective of this project is to analyze the Mudra Loan dataset, clean the data, handle missing values, remove duplicates, and visualize key aspects of the data using box plots. The primary goal is to demonstrate understanding and application of basic data manipulation and visualization techniques.

## Project Overview

In this practical project, I focused on:
1. Converting categorical data to numeric using one-hot encoding.
2. Handling missing values by replacing them with the mean (for numerical data) or mode (for categorical data).
3. Identifying and removing duplicate rows.
4. Visualizing numerical data distributions through box plots.

## Steps Involved

### 1. Data Loading
The dataset was loaded from an online source using `pandas.read_csv()` to start the analysis.

### 2. Data Cleaning and Preprocessing
- **Converting Categorical Data to Numeric**: Used one-hot encoding to convert categorical features into numerical format, making them easier to work with.
- **Handling Missing Data**: Checked for missing values and filled them with:
   - **Mean** for numerical columns.
   - **Mode** for categorical columns.
- **Removing Duplicates**: Checked for duplicate rows in the dataset and removed them to ensure the data’s integrity.

### 3. Data Visualization
Created box plots for each numerical column to understand the data distribution, check for outliers, and assess the spread of values.

## Libraries Used

- `pandas`: To manipulate and analyze the dataset.
- `matplotlib`: For plotting graphs.
- `seaborn`: For creating enhanced visualizations, especially box plots.

## Code Walkthrough

```python
# Import necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: Load the dataset
url = "https://raw.githubusercontent.com/RohitSingh218/MudraLoanDataset/main/mudraloandataset.csv"
df = pd.read_csv(url)

# Step 2: Convert categorical data to numeric using one-hot encoding
categorical_columns = df.select_dtypes(include=['object']).columns
df = pd.get_dummies(df, columns=categorical_columns, drop_first=True)

# Step 3: Handle missing values
for column in df.columns:
    if df[column].isnull().sum() > 0:
        if df[column].dtype in ['float64', 'int64']:
            df[column].fillna(df[column].mean(), inplace=True)
        elif df[column].dtype == 'object':
            df[column].fillna(df[column].mode()[0], inplace=True)

# Step 4: Remove duplicates from the dataset
duplicates = df.duplicated()
df = df[~duplicates]
df.reset_index(drop=True, inplace=True)

# Step 5: Plot box plots for numerical columns
numerical_columns = df.select_dtypes(include=['float64', 'int64']).columns
plt.figure(figsize=(15, 10))
for i, col in enumerate(numerical_columns, 1):
    plt.subplot((len(numerical_columns) + 1) // 2, 2, i)
    sns.boxplot(y=df[col])
    plt.title(f"Box Plot of {col}")
plt.tight_layout()
plt.show()
