# Patient Consultation Data Analysis

## Overview

This project involves a comprehensive analysis of patient consultation data stored in a JSON file. The primary objectives are to clean and transform the data, handle missing values, and extract meaningful insights. The analysis encompasses calculating various statistics such as the percentage of missing values, the percentage of female patients, the count of adult patients, the average number of medicines prescribed, and the distribution of active and inactive medicines. Additionally, the project involves validating phone numbers and examining the correlation between the number of prescribed medicines and patient age.

## Table of Contents

1. Introduction
2. Project Setup
3. Data Loading and Exploration
4. Data Normalization
5. Handling Missing Values
6. Gender Imputation
7. Age Group Categorization
8. Medicine Prescription Analysis
9. Phone Number Validation
10. Correlation Analysis
11. Results Summary
12. Future Scope

## 1. Introduction

This section provides an overview of the project, including its objectives, methodology, and expected outcomes. It outlines the importance of data analysis in improving patient care and operational efficiency in healthcare settings.

## 2. Project Setup

### Prerequisites

Ensure you have the required Python packages installed. This project relies on libraries such as `pandas`, `numpy`, and `matplotlib`.

```bash
pip install pandas numpy matplotlib
```

### Directory Structure

Organize your project files in the following structure:

```
patient-consultation-analysis/
│
├── data/
│   └── patient_data.json
├── notebooks/
│   └── analysis.ipynb
├── scripts/
│   └── data_processing.py
└── README.md
```

## 3. Data Loading and Exploration

### Loading the Data

Load the JSON data file and explore its structure to understand the various fields and data types.

```python
import pandas as pd
import json

# Load the JSON data
with open('data/patient_data.json') as f:
    data = json.load(f)

# Convert to DataFrame
df = pd.json_normalize(data)
df.head()
```

### Exploring the Data

Perform initial exploratory data analysis (EDA) to get a sense of the data distribution and identify any obvious issues.

```python
# Display basic information about the dataset
df.info()

# Summary statistics
df.describe()
```

## 4. Data Normalization

Normalize the JSON data to a flat table for easier analysis. This involves transforming nested JSON structures into a tabular format.

```python
# Normalize nested JSON data
df = pd.json_normalize(data, sep='_')
df.head()
```

## 5. Handling Missing Values

Analyze and handle missing values in the dataset. This includes identifying the percentage of missing values for each column and deciding on appropriate imputation strategies.

```python
# Calculate percentage of missing values
missing_values = df.isnull().mean() * 100
missing_values.sort_values(ascending=False)
```

## 6. Gender Imputation

Impute missing gender values using the mode (most frequent value) to maintain the dataset's integrity.

```python
# Impute missing gender values with the mode
mode_gender = df['gender'].mode()[0]
df['gender'].fillna(mode_gender, inplace=True)
```

## 7. Age Group Categorization

Categorize patients into different age groups based on their birth dates. This can help in understanding age-related patterns in the data.

```python
# Convert birth date to datetime
df['birth_date'] = pd.to_datetime(df['birth_date'])

# Calculate age
df['age'] = (pd.to_datetime('today') - df['birth_date']).dt.days // 365

# Categorize age groups
age_bins = [0, 18, 35, 50, 65, 100]
age_labels = ['Child', 'Young Adult', 'Adult', 'Middle-aged', 'Senior']
df['age_group'] = pd.cut(df['age'], bins=age_bins, labels=age_labels)
```

## 8. Medicine Prescription Analysis

Analyze the prescription data to understand the average number of medicines prescribed and the distribution of active and inactive medicines.

```python
# Calculate average number of medicines prescribed
avg_medicines = df['medicines'].apply(len).mean()

# Distribution of active and inactive medicines
active_medicines = df['medicines'].apply(lambda x: sum(m['active'] for m in x))
inactive_medicines = df['medicines'].apply(lambda x: sum(not m['active'] for m in x))
```

## 9. Phone Number Validation

Validate the phone numbers to ensure they follow the correct format. This can involve using regular expressions to check for common phone number patterns.

```python
import re

# Function to validate phone number
def validate_phone_number(phone):
    pattern = re.compile(r'^\+?[1-9]\d{1,14}$')
    return bool(pattern.match(phone))

# Apply validation function
df['phone_valid'] = df['phone'].apply(validate_phone_number)
```

## 10. Correlation Analysis

Examine the correlation between the number of prescribed medicines and patient age to uncover any potential patterns.

```python
# Calculate the number of medicines prescribed
df['num_medicines'] = df['medicines'].apply(len)

# Correlation between age and number of medicines
correlation = df[['age', 'num_medicines']].corr()
```

## 11. Results Summary

Summarize the key findings from the analysis, including statistics on missing values, gender distribution, age group categorization, medicine prescription patterns, and phone number validation results. Highlight any significant correlations discovered.

## 12. Future Scope

### Enhancements

- **Machine Learning Models:** Implement predictive models to forecast patient outcomes based on consultation data.
- **Advanced Imputation Techniques:** Use machine learning algorithms to impute missing values more accurately.
- **Visualization:** Create interactive dashboards for better data visualization and insights.
- **Integration with Other Data Sources:** Combine this dataset with other healthcare data sources for a more comprehensive analysis.

### Long-Term Goals

- **Real-Time Data Analysis:** Develop systems for real-time analysis and monitoring of patient consultations.
- **Patient Risk Assessment:** Use the data to build risk assessment models for predicting patient health risks and recommending preventive measures.
- **Enhanced Data Security:** Implement advanced data security measures to protect patient information.

## Conclusion

This project demonstrates a structured approach to analyzing patient consultation data, providing valuable insights that can improve healthcare delivery and patient outcomes. By expanding on the current analysis and integrating advanced techniques, we can further enhance the utility and impact of this data.

```bash
# Running the data processing script
python scripts/data_processing.py
```

