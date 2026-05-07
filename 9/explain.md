# Explanation of Data Visualization II Practical

# Aim

Use Titanic dataset to:

1. Create a box plot for age distribution with respect to gender and survival
2. Observe statistical patterns from the visualization

---

# Step-by-Step Code Explanation

# 1. Import Libraries

```python id="0vq1lf"
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## `pandas`

Used for:

* loading dataset
* data analysis
* handling rows and columns

Example:

```python id="j0ul2r"
pd.read_csv()
```

---

## `matplotlib.pyplot`

Used for:

* plotting graphs
* displaying figures

Example:

```python id="6gcuzr"
plt.show()
```

---

## `seaborn`

Used for:

* statistical visualization
* attractive plots
* boxplot creation

---

# 2. Load Dataset

```python id="ydimxr"
df = pd.read_csv("titanic.csv")
```

---

## Explanation

Reads Titanic CSV file into dataframe `df`.

ASCII idea:

```text id="mj7t4q"
Titanic CSV File
        ↓
Pandas DataFrame (df)
```

Now:

* rows = passengers
* columns = passenger details

---

# 3. Display First Rows

```python id="rjg3jn"
print(df.head())
```

---

## Purpose

Displays first 5 rows.

Helps verify:

* dataset loaded correctly
* column names exist

Example columns:

```text id="rm4p4x"
Sex
Age
Survived
Fare
Pclass
```

---

# 4. Dataset Information

```python id="dzkd68"
df.info()
```

---

## Purpose

Shows:

* total rows
* total columns
* datatype
* missing values

Example:

```text id="j9gs1r"
Age -> float
Sex -> object
Survived -> int
```

---

# 5. Missing Values

```python id="5v7x1u"
df.isnull().sum()
```

---

## Purpose

Checks missing values in each column.

Example:

```text id="ed2hm9"
Age -> 177 missing
Cabin -> many missing
```

Important because:

* missing values affect visualizations

---

# 6. Set Seaborn Style

```python id="xyscv6"
sns.set_style("whitegrid")
```

---

## Purpose

Improves graph appearance.

Adds:

* white background
* grid lines

Makes graph easier to read.

---

# 7. Create Figure Size

```python id="g5om9h"
plt.figure(figsize=(10,6))
```

---

## Explanation

Sets graph size.

```text id="9fc0y5"
Width = 10
Height = 6
```

Larger size improves readability.

---

# 8. Box Plot Creation

```python id="nnxjq2"
sns.boxplot(
    x='Sex',
    y='Age',
    hue='Survived',
    data=df
)
```

---

# Most Important Part

This creates the complete boxplot.

---

# Understanding Each Parameter

---

## `x='Sex'`

Separates graph horizontally into:

* male
* female

ASCII idea:

```text id="9q2db8"
Male        Female
```

---

## `y='Age'`

Plots passenger ages vertically.

Higher values:

* older passengers

Lower values:

* younger passengers

---

## `hue='Survived'`

Further divides each gender into:

* survived (`1`)
* not survived (`0`)

So each gender gets:

* survivor boxplot
* non-survivor boxplot

ASCII idea:

```text id="2z0ys6"
Male:
   Survived
   Not Survived

Female:
   Survived
   Not Survived
```

---

## `data=df`

Uses dataframe `df`.

---

# 9. Title and Labels

```python id="brp7ux"
plt.title("Age Distribution by Gender and Survival")
plt.xlabel("Gender")
plt.ylabel("Age")
```

---

## Purpose

Adds:

* graph title
* x-axis label
* y-axis label

Improves understanding.

---

# 10. Display Plot

```python id="0rjuz0"
plt.show()
```

---

## Purpose

Displays graph on screen.

Without this:

* graph may not appear

---

# Understanding Box Plot

ASCII structure:

```text id="k5v3vb"
          Outlier
             •

-----|-------------|-----
    Q1    Median    Q3
```

---

# Box Plot Components

| Component     | Meaning              |
| ------------- | -------------------- |
| Lower whisker | Minimum value        |
| Q1            | 25% values below     |
| Median line   | Middle value         |
| Q3            | 75% values below     |
| Upper whisker | Maximum normal value |
| Dots          | Outliers             |

---

# Example from Your Data

For females:

```text id="7i2k2h"
25% = 18
Median = 27
75% = 37
```

Meaning:

* 25% females below age 18
* middle age = 27
* most between 18–37

---

# 11. Survival Count Analysis

```python id="1lkht3"
df.groupby('Sex')['Survived'].value_counts()
```

---

# Breakdown

---

## `groupby('Sex')`

Groups data into:

* male
* female

---

## `['Survived']`

Selects survival column.

Values:

```text id="5ynckm"
0 = died
1 = survived
```

---

## `value_counts()`

Counts occurrences.

---

# Your Output

```text id="h9xkn8"
female survived = 233
female died = 81

male survived = 109
male died = 468
```

---

# Inference

Females survived more.

---

# 12. Age Statistics by Gender

```python id="jvs8x4"
df.groupby('Sex')['Age'].describe()
```

---

# `describe()` Gives

| Statistic | Meaning            |
| --------- | ------------------ |
| count     | total values       |
| mean      | average            |
| std       | standard deviation |
| min       | minimum            |
| 25%       | first quartile     |
| 50%       | median             |
| 75%       | third quartile     |
| max       | maximum            |

---

# Your Output Example

```text id="zjq2i5"
male mean age = 30.72
female mean age = 27.91
```

---

# Standard Deviation

```text id="ofnkg5"
male std = 14.67
female std = 14.11
```

Higher std means:

* more spread
* more variation

So male ages vary more.

---

# 13. Age Statistics by Survival

```python id="s1smzy"
df.groupby('Survived')['Age'].describe()
```

---

# Purpose

Compares:

* survivor ages
* non-survivor ages

---

# Your Output

```text id="fyk99u"
Survivors mean age = 28.34
Non-survivors mean age = 30.62
```

---

# Inference

Survivors were slightly younger on average.

---

# Final Inferences Explained

| Inference                  | Why It Is Correct       |
| -------------------------- | ----------------------- |
| Females survived more      | Higher survival counts  |
| Most were adults           | Median around 27–29     |
| Male ages spread more      | Higher std deviation    |
| Outliers exist             | Extreme ages present    |
| Survivors slightly younger | Lower survivor mean age |

---

# Overall Conclusion

Using boxplot and statistical analysis:

* survival patterns by gender were identified
* age distribution was analyzed
* outliers were detected
* statistical spread and median values were studied
* females had significantly higher survival rates
