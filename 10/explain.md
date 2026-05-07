# Complete Explanation of the Data Visualization Practical

This practical belongs to:

Data Visualization

using the Iris Dataset.

---

# Main Objective

Understand the dataset visually using:

* histograms
* boxplots
* outlier analysis

This practical is about:

```text id="jlwmc0"
understanding data
NOT machine learning
```

---

# What the Dataset Contains

Each row represents one flower.

The dataset contains flower measurements.

---

# Columns in Dataset

| Column       | Meaning         |
| ------------ | --------------- |
| sepal.length | Length of sepal |
| sepal.width  | Width of sepal  |
| petal.length | Length of petal |
| petal.width  | Width of petal  |
| variety      | Flower species  |

---

# Datatype Understanding

| Column       | Type                 |
| ------------ | -------------------- |
| sepal.length | Numerical Continuous |
| sepal.width  | Numerical Continuous |
| petal.length | Numerical Continuous |
| petal.width  | Numerical Continuous |
| variety      | Nominal Categorical  |

---

# What is Continuous Numerical Data?

Values can contain decimals.

Example:

```text id="jlwmu7"
5.1
3.5
1.4
```

---

# What is Nominal Data?

Categories without ordering.

Example:

```text id="jlwm9x"
Setosa
Versicolor
Virginica
```

No ranking exists between them.

---

# Complete Workflow

```text id="jlwmjv"
Load Dataset
      ↓
Check Dataset Structure
      ↓
Identify Feature Types
      ↓
Create Histograms
      ↓
Create Boxplots
      ↓
Detect Outliers
      ↓
Draw Inference
```

---

# Step 1: Import Libraries

```python id="jlwmf7"
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

# Why Pandas?

```python id="jlwmtm"
import pandas as pd
```

Used for:

* loading dataset
* handling rows and columns

Think of it like Excel inside Python.

---

# Why Matplotlib?

```python id="jlwm1m"
import matplotlib.pyplot as plt
```

Used for:

* creating graphs
* plotting histograms

---

# Why Seaborn?

```python id="jlwms9"
import seaborn as sns
```

Used for:

* better visualizations
* boxplots

---

# Step 2: Load Dataset

```python id="jlwmwr"
df = pd.read_csv("iris.csv")
```

---

# What Happens Here?

Reads CSV file into dataframe.

---

# DataFrame

A table-like structure containing:

* rows
* columns

---

# Step 3: Display First 5 Rows

```python id="jlwmv8"
print(df.head())
```

---

# Purpose

Used to:

* verify dataset loaded correctly
* inspect column names
* inspect sample values

---

# Example Output

```text id="jlwm90"
5.1  3.5  1.4  0.2  Setosa
```

---

# Step 4: Dataset Information

```python id="jlwm8d"
print(df.info())
```

---

# What info() Shows

* total rows
* total columns
* datatype
* null values

---

# Example Understanding

```text id="jlwmiz"
float64
```

means:

* decimal numerical values

---

# Step 5: Display Features and Datatypes

```python id="jlwmdo"
for col in df.columns:
```

---

# What This Loop Does

Goes through every column one-by-one.

---

# Output Example

```text id="jlwmv0"
sepal.length : float64
variety : object
```

---

# Why Important?

Helps identify:

* numerical columns
* categorical columns

before visualization.

---

# Step 6: Numerical Columns List

```python id="jlwm8m"
numerical_cols = [...]
```

---

# Why Created?

Histograms and boxplots work properly only for:

* numerical continuous data

---

# Why variety Excluded?

Because:

```text id="jlwm9m"
variety
```

is categorical text.

Creating histogram/boxplot for flower names is not meaningful.

---

# Step 7: Histograms

```python id="jlwmdf"
plt.hist()
```

---

# What is Histogram?

A graph showing:

```text id="jlwmq1"
frequency distribution
```

of numerical data.

---

# Meaning of Frequency

How many values fall into a range.

---

# Histogram Structure

```text id="jlwm6n"
Tall bar → many values
Short bar → fewer values
```

---

# bins=15

```python id="jlwmvq"
bins=15
```

Divides data into:

* 15 intervals/groups

---

# Example

For sepal length:

```text id="jlwm3v"
4.0–4.5
4.5–5.0
5.0–5.5
```

Each becomes one bin.

---

# What Histograms Help You Understand

## 1. Distribution Shape

Whether data is:

* symmetric
* skewed

---

# 2. Data Spread

Whether values are:

* concentrated
* widely spread

---

# 3. Multiple Peaks

Petal length often shows:

```text id="jlwmgt"
multiple clusters
```

because different flower species differ greatly.

---

# Step 8: Boxplots

```python id="jlwm3z"
sns.boxplot()
```

---

# What is Boxplot?

A statistical graph showing:

* median
* quartiles
* spread
* outliers

---

# Boxplot Structure

```text id="jlwmpy"
Minimum ─ Box ─ Median ─ Box ─ Maximum
```

---

# Middle Line

Represents:

```text id="jlwm4j"
median
```

---

# Box Represents

Middle 50% data.

---

# Whiskers

Show normal data range.

---

# Dots Outside Whiskers

Represent:

```text id="jlwm3x"
potential outliers
```

---

# Why Boxplots Important?

They quickly show:

* spread
* variation
* unusual values

---

# Step 9: Outlier Detection Using IQR

```python id="jlwmfj"
Q1 = df[col].quantile(0.25)
Q3 = df[col].quantile(0.75)
```

---

# What Are Quartiles?

| Term | Meaning             |
| ---- | ------------------- |
| Q1   | 25% data below this |
| Q3   | 75% data below this |

---

# IQR Formula

```python id="jlwmqs"
IQR = Q3 - Q1
```

---

# Meaning

Measures spread of middle 50% data.

---

# Outlier Limits

```python id="jlwml0"
lower_limit = Q1 - 1.5 * IQR
upper_limit = Q3 + 1.5 * IQR
```

---

# Logic

Values outside limits:

* considered statistical outliers

---

# Example

If:

```text id="jlwmd8"
upper limit = 7.5
```

then:

```text id="jlwm5f"
8.2
```

becomes an outlier.

---

# Finding Outliers

```python id="jlwm3m"
outliers = df[
    (df[col] < lower_limit) |
    (df[col] > upper_limit)
]
```

---

# What This Does

Filters rows containing extreme values.

---

# len(outliers)

```python id="jlwm8v"
len(outliers)
```

Counts total outliers.

---

# Important Concept

```text id="jlwm5c"
Outlier ≠ wrong data
```

In iris dataset:

* many outliers are valid flower measurements

---

# Step 10: Inference Section

```python id="jlwm7l"
print("""
...
""")
```

---

# Purpose

Provides final conclusions from visualization.

---

# Inference Meaning

## “All feature columns are continuous numerical variables”

Means:

* measurements contain decimal values

---

## “variety is nominal categorical”

Means:

* flower names are categories

---

## “Histograms show distribution”

Means:

* how values spread across ranges

---

## “Boxplots detect outliers”

Means:

* visually identify extreme values

---

# Why Outliers NOT Removed?

Because practical only asks:

```text id="jlwmbn"
identify outliers
```

NOT:

```text id="jlwmt2"
clean dataset
```

So preprocessing/removal unnecessary.

---

# Final Understanding

This practical teaches:

* dataset understanding
* statistical visualization
* graphical interpretation
* outlier identification

NOT machine learning training.

---

# Final Workflow Diagram

```text id="jlwmtf"
Dataset
   ↓
Feature Analysis
   ↓
Datatype Identification
   ↓
Histogram Visualization
   ↓
Boxplot Visualization
   ↓
Outlier Detection
   ↓
Inference
```

---

# Important Viva Questions

## What is Histogram?

A graph showing frequency distribution of numerical data.

---

## What is Boxplot?

A graph showing:

* median
* spread
* quartiles
* outliers

---

## Why Histograms Only for Numerical Columns?

Because histograms require continuous numerical values.

---

## Why variety Not Used in Boxplot?

Because it is categorical data.

---

## What Are Outliers?

Values significantly far from normal range.

---

## Why Use IQR Method?

Because it is simple and effective for statistical outlier detection.
