# Explanation of Program 1

# Summary Statistics of `data.csv`

---

# Step 1: Import Libraries

```python
import pandas as pd
import numpy as np
```

## What This Does

* `pandas` → used to work with tables/datasets.
* `numpy` → used for numerical operations.

---

# Step 2: Load Dataset

```python
df = pd.read_csv("data.csv")
```

## What Happens

* Reads the CSV file.
* Stores data inside variable `df`.

`df` means **DataFrame**.

A DataFrame is like an Excel table.

ASCII Diagram:

```text
+--------------------------------------------------+
| PassengerId | Sex | Age | Fare | Survived | ... |
+--------------------------------------------------+
|      1      | male| 22  | 7.25 |     0    | ... |
|      2      |female|38  |71.28 |     1    | ... |
+--------------------------------------------------+
```

---

# Step 3: Display First 5 Rows

```python
print(df.head())
```

## Purpose

Shows first 5 rows of dataset.

Used to:

* verify data loaded correctly
* understand columns
* inspect values

Example Output:

```text
PassengerId  Sex     Age   Fare
1            male    22    7.25
2            female  38    71.28
```

---

# Step 4: Dataset Information

```python
print(df.info())
```

## Purpose

Displays:

* column names
* data types
* missing values

Example:

```text
Age     float64
Sex     object
Fare    float64
```

---

# Understanding Column Types

## Numeric Columns

Contain numbers:

| Column | Meaning          |
| ------ | ---------------- |
| Age    | passenger age    |
| Fare   | ticket fare      |
| Pclass | passenger class  |
| SibSp  | siblings/spouse  |
| Parch  | parents/children |

These are used for calculations.

---

## Categorical Columns

Contain labels/categories:

| Column   | Example     |
| -------- | ----------- |
| Sex      | male/female |
| Embarked | S/C/Q       |

These are used for grouping.

---

# Step 5: Remove Missing Values

```python
df = df.dropna(subset=['Age', 'Fare'])
```

## Why Needed

Some passengers have missing ages.

Example:

```text
Age = NaN
```

`NaN` means:

```text
Not a Number
```

Statistics like mean cannot properly work with missing data.

So rows with missing Age or Fare are removed.

---

# Step 6: Group Data by Category

```python
grouped_data = df.groupby('Sex')
```

## What Happens

Dataset splits into groups:

ASCII Diagram:

```text
Entire Dataset
       |
       v
+-------------+
| Group by Sex|
+-------------+
   /       \
Male     Female
```

Now statistics are calculated separately.

---

# Step 7: Calculate Summary Statistics

```python
summary_stats = grouped_data[['Age', 'Fare']].agg(
    ['mean', 'median', 'min', 'max', 'std']
)
```

---

# Understanding Each Statistic

---

## Mean

### Formula

```text
Mean = Sum of values / Number of values
```

Example:

```text
Ages = 20, 30, 40

Mean = (20+30+40)/3
     = 30
```

Shows average.

---

## Median

Middle value after sorting.

Example:

```text
10, 20, 30, 40, 50
Median = 30
```

If even numbers:

```text
10, 20, 30, 40
Median = (20+30)/2
       = 25
```

---

## Minimum

Smallest value.

Example:

```text
2, 5, 8
Min = 2
```

---

## Maximum

Largest value.

Example:

```text
2, 5, 8
Max = 8
```

---

## Standard Deviation (std)

Measures data spread.

---

### Small STD

Values close together.

```text
20, 21, 22
```

Low variation.

---

### Large STD

Values far apart.

```text
5, 50, 90
```

High variation.

---

# Step 8: Display Statistics

```python
print(summary_stats)
```

## Example Output

```text
             Age                              Fare
            mean median min max std
Sex
female     27.9   27    2   63  14
male       30.7   29    1   80  15
```

---

# Reading This Output

## Female Passengers

| Statistic       | Meaning            |
| --------------- | ------------------ |
| mean age = 27.9 | average female age |
| median = 27     | middle female age  |
| min = 2         | youngest female    |
| max = 63        | oldest female      |

---

# Step 9: Create Lists

```python
male_age = df[df['Sex'] == 'male']['Age'].tolist()
```

---

# Breaking It Down

## Condition

```python
df['Sex'] == 'male'
```

Finds male passengers.

---

## Select Age Column

```python
['Age']
```

Gets ages only.

---

## Convert to Python List

```python
.tolist()
```

Converts column into:

```python
[22, 35, 54, 2, 20]
```

Same done for females.

---

# Why Lists are Created

Assignment requirement says:

> Create a list that contains numeric value for each response to categorical variable.

So:

* one list for males
* one list for females

---

# Overall Flow of Program 1

```text
Load Dataset
      |
Check Data Types
      |
Remove Missing Values
      |
Group by Category
      |
Calculate Statistics
      |
Create Numeric Lists
      |
Display Results
```

---

# Explanation of Program 2

# Statistical Details of `iris.csv`

---

# Step 1: Import Libraries

```python
import pandas as pd
import numpy as np
```

Same purpose:

* pandas → dataset handling
* numpy → numerical operations

---

# Step 2: Load Dataset

```python
df = pd.read_csv("iris.csv")
```

Loads iris flower dataset.

---

# Step 3: Display First 5 Rows

```python
print(df.head())
```

Example:

```text
sepal.length  sepal.width  petal.length  variety
5.1           3.5          1.4           Setosa
```

---

# Step 4: Dataset Information

```python
print(df.info())
```

---

# Understanding Iris Columns

| Column       | Type        |
| ------------ | ----------- |
| sepal.length | Numeric     |
| sepal.width  | Numeric     |
| petal.length | Numeric     |
| petal.width  | Numeric     |
| variety      | Categorical |

---

# What is Sepal and Petal?

ASCII Diagram:

```text
        Petal
          /\
         /  \
    -----    -----
      \      /
       \____/
        Sepal
```

---

# Step 5: Display Species

```python
print(df['variety'].unique())
```

## Purpose

Find unique flower types.

Output:

```text
Setosa
Versicolor
Virginica
```

---

# Step 6: Group Dataset by Species

```python
grouped_data = df.groupby('variety')
```

Dataset divided into species groups.

ASCII Diagram:

```text
Iris Dataset
      |
+-----+------+------+
|            |      |
Setosa  Versicolor Virginica
```

---

# Step 7: Calculate Statistics

```python
stats = grouped_data.agg(
    ['mean', 'median', 'std', 'min', 'max']
)
```

Calculates statistics for:

* sepal length
* sepal width
* petal length
* petal width

for each flower species separately.

---

# Example Understanding

Suppose Setosa petal lengths are:

```text
1.4, 1.5, 1.3
```

Then:

| Statistic | Value    |
| --------- | -------- |
| Mean      | average  |
| Median    | middle   |
| Min       | smallest |
| Max       | largest  |

---

# Step 8: Display Statistics

```python
print(stats)
```

Shows grouped statistics.

Example:

```text
Setosa
Mean Sepal Length = 5.0
```

---

# Step 9: Percentiles

```python
quantile([0.25, 0.50, 0.75])
```

---

# What Percentiles Mean

---

## 25th Percentile

25% values are below this.

---

## 50th Percentile

Median.

---

## 75th Percentile

75% values are below this.

---

# Example

Sorted values:

```text
1 2 3 4 5 6 7 8
```

| Percentile | Approx Value |
| ---------- | ------------ |
| 25%        | 2            |
| 50%        | 4            |
| 75%        | 6            |

---

# Why Percentiles Matter

They describe distribution.

ASCII Diagram:

```text
Small Values ---- Middle ---- Large Values
      25%          50%          75%
```

---

# Observations from Iris Dataset

---

## Setosa

* Small petals
* Low variation
* Easily identifiable

---

## Versicolor

* Medium petal sizes
* Moderate variation

---

## Virginica

* Largest petals
* Higher measurements overall

---

# Overall Flow of Program 2

```text
Load Dataset
      |
Understand Columns
      |
Find Species
      |
Group by Species
      |
Calculate Statistics
      |
Calculate Percentiles
      |
Display Results
```
# What This Output Means

This output shows the **25%, 50%, and 75% percentile values** for each flower species.

The program grouped flowers by species and then checked how the measurements are distributed.

---

# Structure of Output

Example:

```text id="3rlp0l"
Percentiles for Setosa:
```

means:

```text id="tjlwm5"
Now showing percentile statistics only for Setosa flowers
```

---

# Columns Meaning

| Column       | Meaning         |
| ------------ | --------------- |
| sepal.length | Length of sepal |
| sepal.width  | Width of sepal  |
| petal.length | Length of petal |
| petal.width  | Width of petal  |

These are all numeric measurements.

---

# Rows Meaning

| Row  | Meaning                  |
| ---- | ------------------------ |
| 0.25 | 25th percentile          |
| 0.50 | 50th percentile (Median) |
| 0.75 | 75th percentile          |

---

# Understanding Setosa Output

```text id="g8x0lb"
0.25           4.8        3.200         1.400          0.2
0.50           5.0        3.400         1.500          0.2
0.75           5.2        3.675         1.575          0.3
```

---

# First Row → 25th Percentile

```text id="4wsl2e"
0.25           4.8        3.200         1.400          0.2
```

Means:

| Feature            | Meaning                                    |
| ------------------ | ------------------------------------------ |
| sepal.length = 4.8 | 25% Setosa flowers have sepal length ≤ 4.8 |
| sepal.width = 3.2  | 25% flowers have sepal width ≤ 3.2         |
| petal.length = 1.4 | 25% flowers have petal length ≤ 1.4        |
| petal.width = 0.2  | 25% flowers have petal width ≤ 0.2         |

---

# Second Row → 50th Percentile

```text id="eyvz7d"
0.50           5.0        3.400         1.500          0.2
```

This is the **median**.

Meaning:

| Feature            | Meaning                        |
| ------------------ | ------------------------------ |
| sepal.length = 5.0 | Half the flowers are below 5.0 |
| petal.length = 1.5 | Middle petal length value      |

---

# Third Row → 75th Percentile

```text id="w09wpw"
0.75           5.2        3.675         1.575          0.3
```

Means:

```text id="v8ykgq"
75% of Setosa flowers have values below these numbers
```

Example:

* 75% flowers have petal width ≤ 0.3

---

# Same Logic for Versicolor

```text id="xct35p"
0.50           5.9        2.800          4.35          1.3
```

Means median values are:

* sepal length = 5.9
* petal length = 4.35
* petal width = 1.3

---

# Same Logic for Virginica

```text id="fj3p8u"
0.75         6.900        3.175         5.875          2.3
```

Means:

* 75% Virginica flowers have:

  * sepal length ≤ 6.9
  * petal length ≤ 5.875

---

# Important Observation

Compare petal lengths:

| Species    | Median Petal Length |
| ---------- | ------------------- |
| Setosa     | 1.5                 |
| Versicolor | 4.35                |
| Virginica  | 5.55                |

This shows:

```text id="1psj4r"
Setosa has smallest petals
Virginica has largest petals
```

---

# Visual Understanding

```text id="t1uj4i"
Small ---------------------- Large

25% -------- 50% -------- 75%
```

* 25% → lower values
* 50% → middle values
* 75% → higher values

---

# Why This is Useful

Percentiles help:

* understand spread of data
* compare species
* identify small/medium/large measurements
* analyze distribution without using only averages
