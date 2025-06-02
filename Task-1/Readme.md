# 🧠 Customer Personality Analysis

This repository contains a complete data preprocessing pipeline for a customer marketing dataset. The goal is to clean and prepare the `marketing_campaign.csv` dataset for customer segmentation, campaign response modeling, or behavioral analysis.

---

## 📌 Project Objectives

- Load and explore the dataset  
- Clean missing data and outliers  
- Engineer new features (like age, total_kids)  
- Export the final cleaned dataset  
- Enable downstream analytics and modeling

---

## 📂 Dataset Overview

The dataset (`marketing_campaign.csv`) contains **2240 customer records** and **29 columns**. It includes:

- **Demographics**: `Year_Birth`, `Education`, `Marital_Status`, `Income`
- **Purchases**: `MntWines`, `MntFruits`, `MntMeatProducts`, etc.
- **Campaign Responses**: `AcceptedCmp1–5`, `Response`
- **Other**: `Kidhome`, `Teenhome`, `Dt_Customer`, `Recency`, `Complain`

> 🔎 **Missing Values**: 24 entries missing in the `Income` column

---

## 🛠️ Preprocessing Steps

---

### 1️⃣ Import Required Libraries

```python
import pandas as pd
import numpy as np
```


### 2️⃣ Load the Dataset
```python
df = pd.read_csv('marketing_campaign.csv', sep='\t')
```


### 3️⃣ Explore the Data
```python
print(df.head())      # First 5 rows
print(df.columns)     # Column names
print(df.info())      # Data types & null values
print(df.shape)       # Dataset shape
```

### 4️⃣ Standardize Column Names
```python
df.columns = df.columns.str.strip().str.replace(' ', '_').str.lower()
```

### 5️⃣ Handle Missing Values
```python
df['income'] = df['income'].fillna(df['income'].median())
print(df.isnull().sum())  # Confirm no missing values
```

### 6️⃣ Convert Date Column to DateTime Format
```python
df['dt_customer'] = pd.to_datetime(df['dt_customer'], format='%d-%m-%Y', errors='coerce')
```

### 7️⃣ Feature Engineering
```python
🧮 Create Age Column
df['age'] = 2025 - df['year_birth']

👨‍👩‍👧‍👦 Create Total Kids Column
df['total_kids'] = df['kidhome'] + df['teenhome']
```

### 8️⃣ Remove Duplicate Records
```python
df = df.drop_duplicates()
```

### 9️⃣ Remove Outliers in Income Column (IQR Method)
```python
q1 = df['income'].quantile(0.25)
q3 = df['income'].quantile(0.75)
iqr = q3 - q1
lower_bound = q1 - 1.5 * iqr
upper_bound = q3 + 1.5 * iqr
df = df[(df['income'] >= lower_bound) & (df['income'] <= upper_bound)]
```
### 🔟 Save Cleaned Dataset to CSV
```python
df.to_csv('Customer_personality_Analysis_Cleaned.csv', index=False)
```
