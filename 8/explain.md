# Explanation of the Practical

## Aim

Use the Titanic dataset with the Seaborn library to:

1. Find patterns in passenger data
2. Visualize fare distribution using a histogram

---

# Step-by-Step Explanation

# 1. Import Libraries

```python id="k8k6az"
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Explanation

### `pandas`

Used to:

* load dataset
* handle rows and columns
* perform analysis

Example:

```python id="7m0g8v"
pd.read_csv()
```

reads CSV file.

---

### `matplotlib.pyplot`

Used for plotting graphs.

Example:

```python id="2fqqdr"
plt.hist()
plt.show()
```

---

### `seaborn`

Advanced visualization library built on matplotlib.

Used for:

* attractive graphs
* statistical visualization
* pattern analysis

---

# 2. Load Dataset

```python id="i3b95n"
df = pd.read_csv("titanic.csv")
```

## Explanation

* Reads Titanic CSV file
* Stores it into dataframe `df`

ASCII idea:

```text id="4qqvcu"
CSV File  ----->  Pandas DataFrame
```

Now:

* rows = passengers
* columns = passenger details

---

# 3. Display Dataset

```python id="q9mt5t"
print(df.head())
```

## Explanation

Displays first 5 rows.

Purpose:

* verify dataset loaded correctly
* understand column names

---

# 4. Dataset Information

```python id="7lw6gq"
df.info()
```

## Explanation

Provides:

* total rows
* total columns
* datatype of each column
* missing values

Example:

```text id="w2g42u"
Age -> float
Sex -> object
Fare -> float
```

---

# 5. Missing Values

```python id="wplj3i"
df.isnull().sum()
```

## Explanation

Checks missing values in each column.

Example:

```text id="7ij0qf"
Age      177
Cabin    687
```

Meaning:

* 177 passengers have missing age
* many cabin values missing

---

# PART 1 — Pattern Analysis using Seaborn

---

# 6. Seaborn Style

```python id="k8wzlx"
sns.set_style("whitegrid")
```

## Explanation

Improves graph appearance.

Adds:

* background grid
* cleaner visualization

---

# 7. Survival Count Plot

```python id="cndg9l"
sns.countplot(x='Survived', data=df)
```

## Explanation

Counts passengers based on survival.

### X-axis

```text id="4d22l9"
0 -> Did not survive
1 -> Survived
```

### Y-axis

Number of passengers.

---

## Pattern Found

More passengers died than survived.

---

ASCII idea:

```text id="gpt5ng"
Passengers
 ^
 | ████████████  (0)
 | ██████        (1)
 +-------------------->
      Died   Survived
```

---

# 8. Survival Based on Gender

```python id="7rf4xk"
sns.countplot(x='Survived', hue='Sex', data=df)
```

## Explanation

### `hue='Sex'`

Separates bars by:

* male
* female

Purpose:

* compare survival between genders

---

## Output Analysis

Your calculated output:

```text id="7h0bgf"
Female survival = 74.20%
Male survival = 18.89%
```

---

## Inference

Female passengers had much higher survival rate.

Reason historically:

```text id="l85tqf"
"Women and children first"
```

policy during rescue.

---

# 9. Passenger Class Distribution

```python id="sgq4o0"
sns.countplot(x='Pclass', data=df)
```

## Explanation

Shows passenger count in:

* Class 1
* Class 2
* Class 3

---

## Pattern Found

Most passengers were in Class 3.

Your analysis confirmed:

```text id="jlwmzt"
Most Common Passenger Class: 3
```

---

# 10. Survival Based on Passenger Class

```python id="9fz4j1"
sns.countplot(x='Survived', hue='Pclass', data=df)
```

## Explanation

Compares:

* survival
* passenger class

---

## Your Actual Results

```text id="31ktht"
Class 1 -> 62.96%
Class 2 -> 47.28%
Class 3 -> 24.23%
```

---

## Inference

First-class passengers had highest survival rate.

Possible reasons:

* better cabin locations
* faster access to lifeboats

---

# 11. Age Distribution

```python id="q74upz"
sns.histplot(df['Age'].dropna(), bins=30, kde=True)
```

---

## Important Part

### `dropna()`

Removes missing ages before plotting.

Without this:

* graph may contain errors or inaccurate results.

---

## `bins=30`

Histogram divided into 30 groups.

More bins:

* more detailed graph

---

## `kde=True`

Adds smooth curve over histogram.

Purpose:

* better visualization of data trend

---

## Your Output Analysis

```text id="sg42tm"
Mean age = 29.7
Median age = 28
```

---

## Inference

Most passengers were adults.

Most ages concentrated around:

```text id="s6hn8p"
20–40 years
```

---

ASCII idea:

```text id="g0x5bh"
Frequency
 ^
 |        ████
 |      ███████
 |    ██████████
 |  █████████
 +-------------------->
   0 20 40 60 80
```

---

# PART 2 — Fare Histogram

# 12. Fare Distribution

```python id="ykyv67"
plt.hist(df['Fare'], bins=30, edgecolor='black')
```

---

## Explanation

Plots histogram of ticket fares.

### X-axis

Ticket Fare

### Y-axis

Number of passengers

---

## Meaning of Histogram

Histogram shows:

* how data values are distributed
* which ranges occur most often

---

## Your Output

```text id="ecdrt9"
Average Fare = 32.20
```

But histogram showed:

* many low fares
* few very high fares

---

# Right-Skewed Distribution

ASCII idea:

```text id="a87qma"
Frequency
 ^
 | ███████████████
 | ███████
 | ███
 | █
 +-------------------->
   Low Fare   High Fare
```

Meaning:

* majority paid low fares
* few rich passengers paid very high fares

---

# Final Inferences Explained

| Inference                       | Why It Is Correct              |
| ------------------------------- | ------------------------------ |
| Females survived more           | Female survival rate higher    |
| Class 1 survived more           | Highest survival percentage    |
| Most fares were low             | Histogram concentrated on left |
| Most passengers were adults     | Mean/median around 30          |
| Most passengers were in Class 3 | Mode = 3                       |

---

# Overall Conclusion

Using Seaborn visualizations and statistical analysis:

* survival patterns were identified
* gender and passenger class strongly affected survival
* fare distribution was uneven
* most passengers belonged to adult age group
* majority traveled in third class
