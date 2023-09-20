# Data Preprocessing and Analysis using Python

## Overview

This Python script performs data preprocessing and analysis tasks on two datasets: "customer details" and "customer policy details." The primary goal is to clean and prepare the data for further analysis and provide insights into the data. Below, we break down the tasks performed and provide explanations for each step.

## Task Breakdown

### 1. Column Names

#### Purpose:
The script starts by assigning meaningful column names to both datasets to ensure consistency and improve readability.

#### Code:
```python
data1.columns= ["customer_id", "Gender", "age", "driving licence present", "region code", "previously insured", "vehicle age", "vehicle damage"]
data2.columns = ["customer_id", "annual premium (in Rs)", "sales channel code", "vintage", "response"]
```

### 2. Data Cleaning and Quality Checks

#### i. Null Values

##### Purpose:
The script identifies and handles null values within the datasets.

##### Code:
- Count null values column-wise:
```python
df1.isnull().sum()
df2.isnull().sum()
```
- Drop null values in "customer_id" column:
```python
df1.dropna(subset=["customer_id"], inplace=True)
df2.dropna(subset=["customer_id"], inplace=True)
```
- Replace null values for numeric columns with mean, and for categorical columns with mode.

#### ii. Outliers

##### Purpose:
The script detects and addresses outliers using the Interquartile Range (IQR) method.

##### Code:
- Calculate IQR and define lower and upper outlier ranges:
```python
def iqr_range(col):
    q1, q3 = np.percentile(col, [25, 75])
    iqr = q3 - q1
    low_iqr_lvl = q1 - (1.5 * iqr)
    high_iqr_lvl = q3 + (1.5 * iqr)
    return [low_iqr_lvl, high_iqr_lvl]

# Check for outliers and replace them with the mean in numeric columns.
```

#### iii. White Spaces and Case Correction
There's no specific code in this script for handling white spaces or performing case correction.

#### iv. Convert to Dummies
There's no code provided for converting nominal data to dummy variables.

#### v. Drop Duplicates
There's no code for dropping duplicate rows in the script.

### 3. Creating a Master Table

#### Purpose:
The script merges the "customer details" and "customer policy details" tables to create a master table using the "customer_id" as the common key.

#### Code:
```python
m_table = pd.merge(df1, df2, on="customer_id")
```

### 4. Important Information

#### Purpose:
The script provides important information that can help the company make future decisions.

#### Code:
- Gender-wise average annual premium:
```python
m_table.groupby(by=["Gender"])["annual premium (in Rs)"].mean()
```
- Age-wise average annual premium:
```python
m_table.groupby(by=["age"])["annual premium (in Rs)"].mean()
```
- Data balance between genders:
```python
m_table.groupby(by=["Gender"]).count()["customer_id"]
```
- Vehicle age-wise average annual premium:
```python
m_table.groupby(by="vehicle age")["annual premium (in Rs)"].mean()
```

### 5. Relationship Between Age and Annual Premium

#### Purpose:
The script explores the relationship between a person's age and their annual premium.

#### Code:
- Calculate the correlation coefficient:
```python
corr = m_table["age"].corr(m_table["annual premium (in Rs)"])
```
- Interpretation of correlation:
    - If the correlation coefficient < -0.5: Strong negative relationship.
    - If the correlation coefficient > 0.5: Strong positive relationship.
    - If -0.5 < correlation coefficient < 0.5: No significant relationship.

## Conclusion

This Python script demonstrates a series of data preprocessing and analysis tasks, making the data ready for further exploration and modeling. The tasks include handling null values, outliers, and merging datasets, followed by providing valuable insights into the data.
