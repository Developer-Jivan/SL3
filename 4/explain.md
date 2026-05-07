# Step 1: Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
```

---

## Explanation

These libraries provide all tools needed for:

* data handling
* numerical operations
* visualization
* machine learning
* model evaluation

---

## Intuition

Think of libraries like specialized toolboxes.

| Library    | Purpose                   |
| ---------- | ------------------------- |
| Pandas     | Handle tables/datasets    |
| NumPy      | Mathematical calculations |
| Matplotlib | Draw graphs               |
| sklearn    | Machine Learning tools    |

Instead of writing everything from scratch, we use ready-made tools.

---

## Concept

### 1. Pandas

`pandas` works with tabular data using a structure called:

```text
DataFrame
```

Internally it stores:

* rows
* columns
* indexes
* datatypes

Similar to:

* Excel sheet
* SQL table

---

### 2. NumPy

NumPy handles:

* arrays
* matrices
* fast numerical operations

Machine learning internally uses arrays heavily.

---

### 3. Matplotlib

Used for:

* scatter plots
* line graphs
* histograms
* visualization

Helps understand:

* trends
* relationships
* predictions

---

### 4. sklearn

`scikit-learn` provides:

* ML algorithms
* preprocessing
* evaluation metrics

Here we use:

* Linear Regression
* Train-Test Split
* Error calculations

---

## Code Mapping

### Pandas

```python
import pandas as pd
```

`pd` becomes shortcut for pandas.

Instead of:

```python
pandas.read_csv()
```

we write:

```python
pd.read_csv()
```

---

### NumPy

```python
import numpy as np
```

Used later for:

```python
np.sqrt(mse)
```

to calculate square root.

---

### Matplotlib

```python
import matplotlib.pyplot as plt
```

`pyplot` gives graph functions.

Used later for:

* scatter plot
* labels
* title

---

### Train Test Split

```python
from sklearn.model_selection import train_test_split
```

Used to divide dataset into:

* training data
* testing data

---

### Linear Regression

```python
from sklearn.linear_model import LinearRegression
```

Imports regression algorithm.

---

### Metrics

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
```

Used to evaluate prediction quality.

---

# Dataset Understanding

Dataset columns:

| Column  | Meaning                             |
| ------- | ----------------------------------- |
| CRIM    | Crime rate in area                  |
| ZN      | Residential land zoning percentage  |
| INDUS   | Industrial area proportion          |
| CHAS    | Charles river dummy variable        |
| NOX     | Nitric oxide concentration          |
| RM      | Average rooms per house             |
| AGE     | Old house proportion                |
| DIS     | Distance from employment centers    |
| RAD     | Highway accessibility               |
| TAX     | Property tax rate                   |
| PTRATIO | Student-teacher ratio               |
| B       | Black population proportion formula |
| LSTAT   | Lower status population percentage  |
| MEDV    | Median home price (TARGET)          |

---

## Important Understanding

### Target Column

```python
MEDV
```

This is what we want to predict.

So:

* INPUT = all other columns
* OUTPUT = MEDV

---

## Numerical vs Categorical Columns

### Numerical Columns

Most columns are continuous numeric values:

```text
CRIM, RM, AGE, TAX, etc.
```

---

### Binary/Categorical-Like Column

```text
CHAS
```

Values:

* 0
* 1

Meaning:

* near river or not

Even though it is numeric, conceptually it behaves like categorical data.

BUT:

* Linear Regression can handle 0/1 directly
* so encoding unnecessary

Hence we DO NOT apply encoding.

That is dataset-specific reasoning.

---

# Step 2: Load Dataset

```python
df = pd.read_csv("housing_data.csv", na_values="NA")
```

---

## Explanation

Loads CSV file into pandas DataFrame.

Also converts `"NA"` into missing values.

---

## Intuition

CSV file is plain text.

Pandas converts it into:

* rows
* columns
* structured table

like Excel inside Python.

---

## Concept

### Why `na_values="NA"`?

Your dataset contains:

```text
NA
```

Example:

```text
0.06905,0,2.18,0,0.458,7.147,54.2,6.0622,3,222,18.7,396.9,NA,36.2
```

`NA` is NOT numeric.

Without this parameter:

* column datatype becomes object/string
* calculations fail

With:

```python
na_values="NA"
```

Pandas converts:

```text
NA → NaN
```

`NaN` means:

```text
Not a Number
```

Now pandas understands:

* value is missing
* numeric operations possible

---

## Code Mapping

```python
pd.read_csv()
```

reads CSV file.

---

### Parameters

| Parameter            | Meaning                     |
| -------------------- | --------------------------- |
| `"housing_data.csv"` | file name                   |
| `na_values="NA"`     | convert NA to missing value |

---

# Step 3: Display First 5 Rows

```python
print(df.head())
```

---

## Explanation

Shows first 5 rows.

---

## Intuition

Before working on data:

* inspect dataset
* check columns
* check formatting

Like checking ingredients before cooking.

---

## Concept

`head()` default:

```python
head(5)
```

Shows top 5 rows.

Useful for:

* quick inspection
* detecting errors
* understanding structure

---

## Code Mapping

```python
df.head()
```

returns first rows.

---

# Step 4: Dataset Information

```python
print(df.info())
```

---

## Explanation

Shows:

* total rows
* total columns
* datatypes
* missing values

---

## Intuition

This is like dataset health report.

---

## Concept

Example output:

```text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 100 entries
Data columns (total 14 columns):
```

---

### Datatype Understanding

Possible datatypes:

| Type    | Meaning        |
| ------- | -------------- |
| int64   | integer        |
| float64 | decimal number |
| object  | string/text    |

---

### Why Important?

Machine learning requires:

* numeric input

If datatype incorrect:

* model fails

---

## Dataset-Specific Reasoning

Because your dataset has:

```text
NA
```

some columns may initially become problematic.

Using:

```python
na_values="NA"
```

helps preserve numeric datatype.

---

# Step 5: Check Missing Values

```python
print(df.isnull().sum())
```

---

## Explanation

Counts missing values column-wise.

---

## Intuition

We want to know:

* where data is incomplete
* which columns need fixing

---

## Concept

### `isnull()`

Converts values into:

| Value         | Result |
| ------------- | ------ |
| Normal value  | False  |
| Missing value | True   |

---

### `.sum()`

True behaves like:

```text
1
```

False behaves like:

```text
0
```

So sum gives count of missing values.

---

## Example

Suppose:

```text
[10, NaN, 20, NaN]
```

After `isnull()`:

```text
[False, True, False, True]
```

After `.sum()`:

```text
2
```

---

# Step 6: Fill Missing Values with Mean

```python
df.fillna(df.mean(numeric_only=True), inplace=True)
```

---

## Explanation

Replaces missing values using column mean.

---

## Intuition

If some house data missing:

* we estimate using average

Example:

```text
Room counts:
5, 6, 7, NA
```

Mean:

```text
6
```

So missing value becomes:

```text
6
```

---

## Why Mean Used?

Dataset-specific reasoning:

Your dataset:

* mostly numerical
* regression problem
* small number of missing values

Mean works well for:

* continuous numeric data

---

## Why NOT Drop Rows?

If we remove rows:

* dataset becomes smaller
* information lost

Only 100 rows available.
So preserving rows better.

---

## Why NOT Mode?

Mode good for:

* categorical columns

This dataset mainly numerical.

---

## Concept

### `fillna()`

Replaces missing values.

---

### `df.mean()`

Computes mean column-wise.

---

### `numeric_only=True`

Ensures:

* only numeric columns considered

Important because mean cannot be calculated on strings.

---

### `inplace=True`

Directly modifies original DataFrame.

Without it:

```python
df = df.fillna(...)
```

would be needed.

---

# Step 7: Verify Missing Values Removed

```python
print(df.isnull().sum())
```

---

## Explanation

Confirms missing values handled successfully.

---

## Expected Output

```text
0
```

for all columns.

---

# Step 8: Outlier Detection & Removal

```python
Q1 = df.quantile(0.25)
Q3 = df.quantile(0.75)

IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

df = df[~((df < lower_bound) | (df > upper_bound)).any(axis=1)]
```

---

# WHY OUTLIER REMOVAL IMPORTANT?

Your dataset contains values like:

```text
MEDV = 43.8
CRIM = 1.61282
```

Some values may be unusually high/low.

Linear Regression is sensitive to outliers because:

* regression line shifts heavily
* predictions become inaccurate

So outlier handling important.

---

# Intuition

Suppose most house prices:

```text
20–30
```

but one house:

```text
200
```

That single value can distort regression line.

---

# Concept: IQR Method

## Quartiles

| Quartile | Meaning  |
| -------- | -------- |
| Q1       | 25% data |
| Q3       | 75% data |

---

## IQR Formula

```text
IQR = Q3 - Q1
```

Measures spread of middle data.

---

## Outlier Limits

### Lower Bound

```text
Q1 - 1.5 × IQR
```

### Upper Bound

```text
Q3 + 1.5 × IQR
```

Values outside bounds:

* considered outliers

---

# Code Mapping

## Q1

```python
Q1 = df.quantile(0.25)
```

25th percentile.

---

## Q3

```python
Q3 = df.quantile(0.75)
```

75th percentile.

---

## IQR

```python
IQR = Q3 - Q1
```

---

## Bounds

```python
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
```

---

## Remove Outliers

```python
df = df[~((df < lower_bound) | (df > upper_bound)).any(axis=1)]
```

### Internally:

#### `(df < lower_bound)`

Checks small outliers.

---

#### `(df > upper_bound)`

Checks large outliers.

---

#### `|`

OR operation.

---

#### `.any(axis=1)`

Checks row-wise:

* if ANY column has outlier

---

#### `~`

Negation operator.

Keeps only normal rows.

---

# Step 9: Separate Features and Target

```python
X = df.drop("MEDV", axis=1)
y = df["MEDV"]
```

---

## Explanation

Separates:

* input features
* target output

---

## Intuition

We teach model:

```text
Using house details → predict price
```

So:

* house details = X
* price = y

---

## Concept

### X

Independent variables/features.

---

### y

Dependent variable/target.

---

## Code Mapping

### Drop Target Column

```python
df.drop("MEDV", axis=1)
```

`axis=1` means:

```text
column operation
```

---

### Target

```python
df["MEDV"]
```

Extracts target column.

---

# Step 10: Train-Test Split

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

---

# Why Split Data?

If model trains AND tests on same data:

* memorization happens
* fake accuracy

Need unseen data testing.

---

# Intuition

Like:

* studying with practice questions
* testing with exam questions

---

# Concept

## 80-20 Split

| Portion | Purpose  |
| ------- | -------- |
| 80%     | training |
| 20%     | testing  |

---

## `random_state=42`

Ensures:

* same split every run
* reproducible results

---

# Step 11: Create Model

```python
model = LinearRegression()
```

---

# Linear Regression Concept

Regression fits equation:

```text
y = mx + c
```

But with many features:

```text
y = b0 + b1x1 + b2x2 + ...
```

Where:

* y = house price
* x = features
* b = coefficients

---

# Goal

Find best-fit line minimizing prediction error.

---

# Step 12: Train Model

```python
model.fit(X_train, y_train)
```

---

# Concept

Model learns:

* relationship between features and prices

Internally calculates:

* coefficients
* intercept

using least squares optimization.

---

# Step 13: Predict

```python
y_pred = model.predict(X_test)
```

---

# Explanation

Uses learned equation to predict prices.

---

# Internal Working

For each test row:

```text
Predicted Price =
b0 + b1x1 + b2x2 + ...
```

---

# Step 14: Model Evaluation

```python
mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_test, y_pred)
```

---

# Metrics Explanation

---

## MAE

### Formula

```text
|Actual - Predicted|
```

Average absolute error.

---

## MSE

Squares errors.

Punishes large mistakes more.

---

## RMSE

Square root of MSE.

Returns error in original units.

---

## R² Score

Measures goodness of fit.

| Value | Meaning |
| ----- | ------- |
| 1     | perfect |
| 0     | poor    |

---

# Step 15: Actual vs Predicted Table

```python
comparison = pd.DataFrame({
    "Actual Price": y_test.values,
    "Predicted Price": y_pred
})
```

---

# Explanation

Compares:

* real prices
* predicted prices

Useful for:

* validation
* understanding model quality

---

# Step 16: Scatter Plot

```python
plt.scatter(y_test, y_pred)
```

---

# Graph Understanding

## X-axis

Actual prices.

---

## Y-axis

Predicted prices.

---

# Good Model Pattern

Points should appear near diagonal line.

Meaning:

```text
Predicted ≈ Actual
```

---

# Viva Questions

## Q1. Why use Linear Regression?

Because target variable:

```text
MEDV
```

is continuous numeric value.

---

## Q2. Why fill missing values with mean?

Dataset mostly numerical.
Mean preserves distribution reasonably.

---

## Q3. Why remove outliers?

Linear Regression highly sensitive to extreme values.

---

## Q4. Why train-test split?

To evaluate model on unseen data.

---

## Q5. Why use R² score?

Measures how well model explains variance.

---

# Common Beginner Mistakes

| Mistake                | Problem              |
| ---------------------- | -------------------- |
| Forgetting NA handling | model error          |
| Including target in X  | data leakage         |
| No train-test split    | fake accuracy        |
| Ignoring outliers      | poor regression line |
| Wrong axis in drop     | runtime error        |

---

# Important Exam Justifications

## Why no encoding?

Dataset already numeric.

---

## Why no normalization?

Linear Regression can still work reasonably without scaling here.
Practical is basic exam-oriented implementation.

---

## Why mean imputation?

Missing values are numerical.

---

## Why IQR method?

Simple and widely used statistical outlier detection technique.
