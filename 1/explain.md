# Data Wrangling using Titanic Dataset

## Aim

To perform Data Wrangling operations on Titanic dataset using Python and Pandas.

Operations performed:
- Import libraries
- Load dataset
- Check missing values
- Study statistics
- Handle missing data
- Normalize numerical data
- Convert categorical data into numerical form

---

# Step 1: Import Required Libraries

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler
```

## Explanation

### Intuition
We import all tools needed for:
- handling tables
- numerical operations
- normalization

Like collecting tools before starting work.

---

### Concept

### Pandas
Used for:
- reading CSV files
- handling rows and columns
- data cleaning

### NumPy
Used for:
- numerical operations
- arrays
- calculations

### MinMaxScaler
Used for normalization.

Normalization converts values into:
\[
0 \rightarrow 1
\]

---

### Code Mapping

```python
import pandas as pd
```
Imports pandas library.

```python
import numpy as np
```
Imports NumPy library.

```python
from sklearn.preprocessing import MinMaxScaler
```
Imports normalization tool.

---

# Step 2: Load Dataset

```python
df = pd.read_csv("data.csv")
```

## Explanation

### Intuition
Load CSV file into Python.

Like opening Excel sheet inside Python.

---

### Concept

`read_csv()`:
- reads CSV file
- converts it into DataFrame

A DataFrame is:
- rows + columns
- table-like structure

---

### Code Mapping

```python
pd.read_csv("data.csv")
```

Reads Titanic dataset.

---

# Step 3: Display First 5 Rows

```python
print(df.head())
```

## Explanation

### Intuition
Quick preview of dataset.

---

### Concept

`head()` returns first 5 rows.

Useful for:
- checking columns
- checking formatting
- verifying successful loading

---

### Expected Output

| PassengerId | Survived | Sex |
|---|---|---|
| 1 | 0 | male |
| 2 | 1 | female |

---

# Step 4: Check Shape of Dataset

```python
print(df.shape)
```

## Explanation

### Intuition
Find:
- total rows
- total columns

---

### Concept

Shape returns:
\[
(rows, columns)
\]

Example:
```python
(20, 12)
```

Means:
- 20 rows
- 12 columns

---

# Step 5: Check Data Types

```python
print(df.dtypes)
```

## Explanation

### Intuition
Understand type of each column.

---

### Concept

| Type | Meaning |
|---|---|
| int64 | Integer |
| float64 | Decimal |
| object | Text/String |

---

### Example

| Column | Type |
|---|---|
| Age | float64 |
| Name | object |
| Fare | float64 |

---

# Step 6: Check Missing Values

```python
print(df.isnull().sum())
```

## Explanation

### Intuition
Find empty values in dataset.

---

### Concept

`isnull()`
checks whether value is missing.

Returns:
- True → missing
- False → available

`sum()`
counts total missing values.

---

### Example

| Column | Missing Values |
|---|---|
| Age | 3 |
| Cabin | 15 |

---

# Step 7: Statistical Summary

```python
print(df.describe())
```

## Explanation

### Intuition
Get quick statistics of numerical columns.

---

### Concept

`describe()` calculates:

| Statistic | Meaning |
|---|---|
| count | total values |
| mean | average |
| std | standard deviation |
| min | minimum |
| max | maximum |

---

### Formula of Mean

\[
Mean = \frac{\text{sum of values}}{\text{number of values}}
\]

---

# Step 8: Handle Missing Values

```python
df['Age'] = df['Age'].fillna(df['Age'].mean())

df['Embarked'] = df['Embarked'].fillna(df['Embarked'].mode()[0])

df.drop('Cabin', axis=1, inplace=True)
```

## Explanation

### Intuition

- Fill missing Age using average
- Fill missing Embarked using most common value
- Remove Cabin because too many values are missing

---

### Concept

### Why Mean for Age?
Age is numerical.

Example:
```python
20, 30, NaN
```

Mean:
\[
25
\]

Missing value becomes 25.

---

### Why Mode for Embarked?
Embarked is categorical.

Mode = most frequent category.

---

### Why Drop Cabin?
Cabin has too many missing values.

Filling it would create unrealistic data.

---

### Code Mapping

```python
fillna()
```
fills missing values.

```python
mean()
```
calculates average.

```python
mode()[0]
```
returns most frequent value.

```python
drop()
```
removes column.

---

# Step 9: Normalize Fare Column

```python
scaler = MinMaxScaler()

df['Fare_Normalized'] = scaler.fit_transform(df[['Fare']])
```

## Explanation

### Intuition
Convert Fare values into range:
\[
0 \rightarrow 1
\]

---

### Concept

Large values can dominate smaller values.

Normalization scales values fairly.

---

### Formula

\[
X_{norm} =
\frac{X - X_{min}}
{X_{max} - X_{min}}
\]

---

### Example

| Original Fare | Normalized |
|---|---|
| 7.25 | 0.00 |
| 71.28 | 1.00 |

---

### Code Mapping

```python
MinMaxScaler()
```
creates scaler object.

```python
fit_transform()
```
learns min/max and transforms values.

---

# Step 10: Convert Categorical Variables to Numerical

## Gender Encoding

```python
df['Sex'] = df['Sex'].map({'male': 0, 'female': 1})
```

## Explanation

### Intuition
Computers understand numbers better than text.

---

### Mapping

| Gender | Number |
|---|---|
| male | 0 |
| female | 1 |

---

## One Hot Encoding

```python
df = pd.get_dummies(df, columns=['Embarked'])
```

## Explanation

### Intuition
Convert categories into separate columns.

---

### Before

| Embarked |
|---|
| S |
| C |

---

### After

| Embarked_S | Embarked_C |
|---|---|
| 1 | 0 |
| 0 | 1 |

---

### Why One Hot Encoding?

If:
```python
S=1
C=2
Q=3
```

model may think:
\[
Q > C > S
\]

which is incorrect.

One Hot Encoding avoids this problem.

---

# Step 11: Display Final Processed Dataset

```python
print(df.head())
```

## Explanation

### Intuition
View cleaned and processed dataset.

---

### Concept
Dataset now contains:
- cleaned values
- normalized data
- numerical categorical variables

Ready for machine learning.

---

# Step 12: Final Missing Values Check

```python
print(df.isnull().sum())
```

## Explanation

### Intuition
Verify whether preprocessing worked correctly.

---

### Concept
If output shows:
```python
0
```

then missing values are handled successfully.

---

# Common Viva Questions

## Q1. What is Data Wrangling?
Process of cleaning and transforming raw data into usable form.

---

## Q2. Why use normalization?
To scale values into common range.

---

## Q3. Why convert categorical variables?
Machine learning algorithms work mainly with numbers.

---

## Q4. Difference between Label Encoding and One Hot Encoding?

| Label Encoding | One Hot Encoding |
|---|---|
| assigns numbers | creates separate columns |
| may create false order | avoids false order |

---

# Conclusion

In this practical we:
- loaded Titanic dataset
- checked missing values
- analyzed statistics
- handled missing data
- normalized Fare column
- converted categorical variables into numerical form
- prepared dataset for analysis and machine learning