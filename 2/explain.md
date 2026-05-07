# Data Wrangling II – Code Explanation

---

# AIM

To perform:

* data cleaning
* inconsistency handling
* outlier detection
* outlier treatment
* data transformation

on an Academic Performance Dataset using Python.

---

# Step 1: Import Libraries

```python id="2sgp3p"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler
```

## Explanation

### Intuition

Libraries are pre-written modules that provide useful functions.

Instead of writing all logic manually:

* we import ready-made tools.

---

## Concept

### `pandas`

Used for:

* reading CSV files
* handling tables
* cleaning datasets

Main structure:

```text id="3l5xgd"
DataFrame
```

A DataFrame is:

* rows + columns
* similar to Excel sheet.

---

### `numpy`

Used for:

* numerical operations
* mathematical calculations
* missing value representation

Example:

```python id="uz1eg5"
np.nan
```

means:

```text id="a7suxw"
missing value
```

---

### `matplotlib.pyplot`

Used for:

* plotting graphs
* creating boxplots
* visualization

---

### `MinMaxScaler`

Used for:

* normalization
* scaling data between 0 and 1

---

## Code Mapping

```python id="m9k15d"
import pandas as pd
```

Imports pandas library.

`pd` is alias.

---

```python id="xg8r4m"
import numpy as np
```

Imports NumPy.

---

```python id="5q5ajk"
import matplotlib.pyplot as plt
```

Imports graph plotting module.

---

```python id="c1y3rx"
from sklearn.preprocessing import MinMaxScaler
```

Imports Min-Max Scaling class.

---

# Step 2: Load Dataset

```python id="8uvd66"
df = pd.read_csv("tecdiv.csv")
```

## Explanation

### Intuition

This step loads dataset into Python memory.

Without loading:

* Python cannot analyze data.

---

## Concept

### `read_csv()`

Reads CSV file and converts it into DataFrame.

Syntax:

```python id="mjlwm9"
pd.read_csv("filename.csv")
```

---

## What is CSV?

CSV means:

```text id="1bm69y"
Comma Separated Values
```

Example:

```text id="xbv5nl"
Name,Marks
Amit,90
Riya,85
```

---

## What is `df`?

`df` means:

```text id="mjlwm9"
DataFrame
```

Stores complete dataset.

---

## Code Mapping

```python id="yffqkz"
pd.read_csv()
```

Reads dataset.

---

```python id="1d76mn"
df =
```

Stores data into variable.

---

# Step 3: Display Dataset Information

```python id="9gj0y5"
print("First 5 Rows:\n")
print(df.head())

print("\nDataset Info:\n")
print(df.info())

print("\nMissing Values:\n")
print(df.isnull().sum())
```

## Explanation

### Intuition

Before cleaning data:

* first inspect it
* understand structure
* identify problems

Never preprocess blindly.

---

# Part 1: `head()`

```python id="6f5x4m"
df.head()
```

## Purpose

Displays first 5 rows.

---

## Why Use It?

Helps verify:

* dataset loaded correctly
* columns are proper
* values look reasonable

---

# Part 2: `info()`

```python id="h6kp27"
df.info()
```

## Purpose

Shows:

* number of rows
* number of columns
* datatype
* non-null values

---

## Important Observation

### Numerical Columns

```text id="nngt7q"
float64
```

Example:

* semester scores

---

### Text Columns

```text id="q6ph3h"
object
```

Example:

* name
* email
* roll number

---

# Part 3: `isnull().sum()`

```python id="0k39ty"
df.isnull().sum()
```

## Purpose

Checks missing values.

---

## How It Works

### `isnull()`

Returns:

* True → missing
* False → not missing

---

### `sum()`

Counts total missing values.

---

## Observation

Initially:

```text id="i1k3w0"
No actual missing values exist
```

But inconsistent values still exist:

* 0
* 95

These are logical inconsistencies.

---

# Step 4: Select Academic Columns

```python id="z1xgny"
academic_cols = [
    'First year:   Sem 1',
    'First year:   Sem 2',
    'Second year:   Sem 1',
    'Second year:   Sem 2'
]
```

## Explanation

### Intuition

We only perform cleaning on academic score columns.

---

## Concept

Not every numerical-looking column should be processed numerically.

---

## Example

### Correct Numerical Variables

| Column          | Reason                          |
| --------------- | ------------------------------- |
| Semester Scores | Measurable academic performance |

---

### Incorrect for Numerical Processing

| Column        | Why Not    |
| ------------- | ---------- |
| Mobile Number | Identifier |
| PRN Number    | Identifier |

---

## Why Store Columns in List?

So we can:

* loop through them
* avoid repetitive code

---

# Step 5: Detect Inconsistent Values

```python id="wl8v4m"
print("\nStatistical Summary:\n")

for col in academic_cols:
    print("\n", col)
    print(df[col].describe())
```

## Explanation

### Intuition

We inspect statistics to detect:

* abnormal values
* inconsistencies
* suspicious entries

---

## Concept

### `describe()`

Provides:

* count
* mean
* standard deviation
* minimum
* maximum
* quartiles

---

# Important Observation

Most SGPA values:

```text id="g5kdr0"
6 to 10
```

But dataset contains:

```text id="w6ztko"
0
95
```

which are suspicious.

---

## Why 95 is Inconsistent?

SGPA normally ranges:

```text id="ow7x5n"
0 to 10
```

So:

```text id="a0v2zq"
95
```

is clearly incorrect.

Likely:

* data entry error.

---

## Why 0 is Treated as Missing?

In this dataset:

* DSE students joined from second year
* they do not have first-year marks

So:

```text id="3itkjz"
0 represents unavailable data
```

rather than actual SGPA.

---

# Step 6: Handle Missing and Inconsistent Values

```python id="mkv59t"
for col in academic_cols:
    
    # Replace 0 with NaN
    df[col] = df[col].replace(0, np.nan)
    
    # Replace values greater than 10 with NaN
    df.loc[df[col] > 10, col] = np.nan
    
    # Fill missing values using median
    median_value = df[col].median()
    df[col] = df[col].fillna(median_value)
```

## Explanation

### Intuition

We:

1. identify invalid values
2. convert them into missing values
3. fill them properly

---

# Part 1: Replace 0 with NaN

```python id="h1i4o3"
df[col] = df[col].replace(0, np.nan)
```

## Purpose

Converts:

```text id="gf6gv5"
0 → missing value
```

---

## Why?

Because DSE students:

* do not have first-year records.

---

# Part 2: Replace Values Greater Than 10

```python id="fgo9sm"
df.loc[df[col] > 10, col] = np.nan
```

## Purpose

Detects impossible SGPA values.

Example:

```text id="twq4sl"
95
```

and converts them into missing values.

---

## Understanding `loc`

### Syntax

```python id="0n1e2y"
df.loc[condition, column]
```

Used for:

* selecting rows conditionally
* modifying values

---

# Part 3: Fill Missing Values

```python id="n00k6j"
median_value = df[col].median()

df[col] = df[col].fillna(median_value)
```

## Why Median?

Median is robust to extreme values.

---

## Example

Values:

```text id="9j3x4m"
7, 8, 9, 95
```

### Mean

```text id="u9lc5f"
29.75
```

Incorrect representation.

---

### Median

```text id="o4lqoj"
8.5
```

Better representation.

---

# Step 7: Boxplot Before Handling Outliers

```python id="6cyvlx"
print("\nBoxplots Before Outlier Handling")

for col in academic_cols:
    plt.figure(figsize=(5, 3))
    plt.boxplot(df[col])
    plt.title(f"Before Outlier Handling - {col}")
    plt.show()
```

## Explanation

### Intuition

We visualize data distribution before removing outliers.

---

# What is Boxplot?

A graph showing:

* spread of data
* median
* quartiles
* outliers

---

# Components of Boxplot

| Part            | Meaning         |
| --------------- | --------------- |
| Box             | Middle 50% data |
| Line inside box | Median          |
| Whiskers        | Normal range    |
| Dots outside    | Outliers        |

---

# Why Use Boxplot?

Advantages:

* easy to interpret
* visually detects outliers
* commonly used in practicals

---

# `figsize=(5,3)`

Controls graph size.

---

# `plt.show()`

Displays graph.

---

# Step 8: Handle Outliers Using IQR Method

```python id="w5d94u"
for col in academic_cols:
    
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    
    IQR = Q3 - Q1
    
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    
    median_value = df[col].median()
    
    df.loc[(df[col] < lower) | (df[col] > upper), col] = median_value
```

## Explanation

### Intuition

Outliers are unusually far values.

We:

1. detect them
2. replace them using median

---

# Quartiles

| Quartile | Meaning  |
| -------- | -------- |
| Q1       | 25% data |
| Q2       | Median   |
| Q3       | 75% data |

---

# IQR Formula

```text id="q0f16g"
IQR = Q3 - Q1
```

Measures spread of middle 50% data.

---

# Outlier Limits

## Lower Limit

```text id="w1r4ek"
Q1 - 1.5 × IQR
```

## Upper Limit

```text id="fdvjlwm"
Q3 + 1.5 × IQR
```

Values outside this range are outliers.

---

# Why Replace Outliers?

Outliers can:

* distort averages
* affect graphs
* impact machine learning

---

# Why Use Median Again?

Median is stable and less sensitive to extreme values.

---

# Step 9: Boxplot After Handling Outliers

```python id="4yduw5"
print("\nBoxplots After Outlier Handling")

for col in academic_cols:
    plt.figure(figsize=(5, 3))
    plt.boxplot(df[col])
    plt.title(f"After Outlier Handling - {col}")
    plt.show()
```

## Explanation

### Intuition

We compare graphs:

* before treatment
* after treatment

to verify outlier removal.

---

# Expected Observation

After treatment:

* extreme points reduce
* distribution becomes cleaner

---

# Step 10: Apply Data Transformation

```python id="34h4fy"
scaler = MinMaxScaler()

df[academic_cols] = scaler.fit_transform(df[academic_cols])
```

## Explanation

### Intuition

Different scales make comparison difficult.

Transformation converts values into common range:

```text id="0xpdz7"
0 to 1
```

---

# What is Min-Max Scaling?

Normalization technique.

---

# Formula

```text id="r65j5g"
X_new = (X - X_min) / (X_max - X_min)
```

---

# Example

Original:

```text id="a2u8x5"
6, 8, 10
```

Scaled:

```text id="6m7rcc"
0, 0.5, 1
```

---

# Why Transformation is Needed?

Practical specifically asks for:

```text id="jlwm3h"
data transformation
```

Purpose:

* scale conversion
* normalization
* better comparison

---

# Why Min-Max Scaling Here?

Because:

* marks are already compact
* data is not heavily skewed
* easy to explain in viva

---

# Step 11: Display Final Dataset

```python id="jlwm3h"
print("\nCleaned and Transformed Dataset:\n")
print(df.head())
```

## Explanation

### Intuition

Displays final processed dataset.

---

## Final Dataset Now Has

* cleaned inconsistencies
* handled missing values
* treated outliers
* transformed academic columns

---

# Overall Workflow Summary

```text id="it65hm"
Raw Data
   ↓
Inspection
   ↓
Inconsistency Detection
   ↓
Missing Value Handling
   ↓
Outlier Detection
   ↓
Outlier Treatment
   ↓
Data Transformation
   ↓
Clean Final Dataset
```

---

# Common Viva Questions

## Q1. What is data wrangling?

Process of:

* cleaning
* transforming
* organizing data

---

## Q2. Why use median instead of mean?

Median is less affected by extreme values.

---

## Q3. What is an outlier?

A value significantly different from normal observations.

---

## Q4. Why use IQR method?

Simple and robust outlier detection method.

---

## Q5. Why apply Min-Max Scaling?

To normalize values into range:

```text id="hh5wq1"
0 to 1
```

---

## Q6. Why were 0 values replaced?

Because DSE students did not have first-year records, so 0 represented unavailable data.
