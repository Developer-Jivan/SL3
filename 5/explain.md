# Logistic Regression on Social_Network_Ads.csv — Full Deep Explanation

## (Concept First + Code First + Viva Ready)

---

# 🔷 MAIN GOAL OF THIS PRACTICAL

We want to predict:

```text id="p8m6q7"
Will customer purchase product?
```

Output:

```text id="s0x4k1"
0 = No
1 = Yes
```

Using customer details:

```text id="r4u9n2"
Age
EstimatedSalary
Gender
```

So this is a:

```text id="t6h3v8"
Supervised Machine Learning
Classification Problem
```

Because output is category (0 or 1)

---

# 🔷 COMPLETE FLOW

```text id="w3a8z5"
Import Libraries
↓
Load Dataset
↓
Check Data
↓
Handle Missing Values
↓
Convert Text to Number
↓
Visualize Data
↓
Split Features / Target
↓
Train-Test Split
↓
Scale Data
↓
Train Logistic Regression
↓
Predict
↓
Evaluate using Confusion Matrix
```

---

# 🔷 SECTION 1 — Import Libraries

```python id="u9c1x6"
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## ✅ Intuition

Libraries are ready-made toolboxes.

Instead of coding graphs, tables, ML manually, we import tools.

---

## ✅ Uses

| Library    | Purpose                     |
| ---------- | --------------------------- |
| NumPy      | Numerical arrays            |
| Pandas     | DataFrame / CSV             |
| Matplotlib | Basic graphs                |
| Seaborn    | Beautiful statistical plots |

---

# 🔷 SECTION 2 — Import sklearn Tools

```python id="m2f7q4"
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder, StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import ...
```

---

## Why sklearn?

Machine learning toolkit.

Contains:

* splitting tools
* preprocessing
* models
* evaluation

---

# 🔷 SECTION 3 — Load Dataset

```python id="g5r8n1"
df = pd.read_csv("Social_Network_Ads.csv")
```

---

# What happens?

CSV file becomes DataFrame table.

Example:

| Age | Salary | Gender | Purchased |
| --- | ------ | ------ | --------- |
| 25  | 30000  | Male   | 0         |

---

# Internally

```text id="f4n2y7"
Rows = observations
Columns = variables/features
```

---

# Viva

> `read_csv()` loads CSV file into pandas DataFrame.

---

# 🔷 SECTION 4 — Basic Info

```python id="j1d8m3"
print(df.head())
print(df.shape)
print(df.isnull().sum())
```

---

# head()

Shows first 5 rows.

---

# shape

```python id="n7s5p2"
(rows, columns)
```

Example:

```text id="a9v6k1"
(400, 5)
```

---

# isnull().sum()

Counts missing values column-wise.

Example:

```text id="b2q4w9"
Age    2
Salary 1
```

---

# Viva

> Used for understanding structure and checking missing data.

---

# 🔷 SECTION 5 — Missing Values

```python id="e6u3z8"
df["Age"] = df["Age"].fillna(df["Age"].mean())
df["EstimatedSalary"] = df["EstimatedSalary"].fillna(df["EstimatedSalary"].mean())
```

---

# Intuition

Blank cells replaced with average.

---

# Example

Age:

```text id="h1m7c5"
20
25
NaN
35
```

Mean:

```text id="k4p9d2"
(20+25+35)/3 = 26.67
```

After fill:

```text id="r8w1j6"
20
25
26.67
35
```

---

# Why mean?

Good simple method for numeric columns.

---

# Viva

> Mean imputation preserves rows and removes null values.

---

# 🔷 SECTION 6 — Label Encoding

```python id="v3n6x1"
le = LabelEncoder()
df["Gender"] = le.fit_transform(df["Gender"])
```

---

# Problem

Machine learning needs numbers.

Cannot understand:

```text id="z7d2q8"
Male
Female
```

---

# Converts To

```text id="x5k9b4"
Female = 0
Male = 1
```

(usually alphabetical)

---

# fit_transform()

## fit()

Learns unique labels.

## transform()

Converts text to number.

---

# Viva

> LabelEncoder converts categorical text data into numeric labels.

---

# 🔷 SECTION 7 — Heatmap

```python id="q2m8w5"
plt.figure(figsize=(6,4))
sns.heatmap(df.corr(), annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap")
plt.show()
```

---

# df.corr()

Finds correlation between numeric columns.

Range:

```text id="l1t7e3"
+1 strong positive
0 no relation
-1 negative relation
```

---

# Example

If Age increases and Purchase increases:

positive relation.

---

# Parameters

## figsize=(6,4)

Width=6, Height=4 inches

## annot=True

Write values inside boxes.

## cmap="coolwarm"

Blue to red color theme.

---

# Read Graph

Dark positive value = stronger relation.

---

# Viva

> Heatmap is used to visualize correlation among features.

---

# 🔷 SECTION 8 — Boxplot (Outlier Detection)

```python id="d5c9n7"
plt.figure(figsize=(15,8))
plt.subplot(1,2,1)
sns.boxplot(y=df["Age"])
```

---

# What is Outlier?

Unusual extreme value.

Example:

```text id="s6f4m1"
22, 25, 26, 28, 130
```

130 is outlier.

---

# Boxplot Parts

```text id="p2y8r5"
whisker | box | whisker
        median
dots outside = outliers
```

---

# subplot(1,2,1)

Means:

```text id="t8n4x6"
1 row
2 columns
graph 1
```

Second graph:

```python id="c4z7k2"
plt.subplot(1,2,2)
```

---

# Why no outliers visible?

Your dataset may be naturally clean.

---

# Viva

> Boxplot helps detect spread, median and outliers.

---

# 🔥 MOST IMPORTANT SECTION — Features and Target

```python id="m8v2p6"
X = df[["Age", "EstimatedSalary", "Gender"]]
y = df["Purchased"]
```

---

# Meaning

## X = Input variables

Used to predict.

## y = Output

What we want.

---

# Internally

```text id="u1k5c9"
X = matrix of rows × columns
y = single output column
```

---

# Example

```text id="w7q3d8"
X = [25, 30000, 1]
y = 0
```

---

# Viva

> Independent variables stored in X, dependent variable stored in y.

---

# 🔥 SECTION — Train Test Split

```python id="a3r7m9"
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=0
)
```

---

# Why split?

Need unseen data to test model.

If tested on same training data:

```text id="n4b6t2"
fake accuracy
```

---

# Example

400 rows:

```text id="j8x2q5"
Train = 300 rows
Test = 100 rows
```

---

# Parameters

## test_size=0.25

25% test data.

## random_state=0

Fixed random split.

Same result every run.

---

# Outputs

```text id="g9k3p1"
X_train = training inputs
X_test  = testing inputs
y_train = training outputs
y_test  = testing outputs
```

---

# Viva

> Data is split into training and testing sets for fair model evaluation.

---

# 🔥 SECTION — Feature Scaling

```python id="e1d5r8"
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)
```

---

# Why needed?

Age:

```text id="f3p7n4"
18 to 60
```

Salary:

```text id="m6r2w9"
15000 to 150000
```

Salary values dominate.

Need common scale.

---

# Formula

```text id="h8t1q6"
z = (x - mean) / std
```

---

# Why fit only train?

```python id="v7n3m5"
fit_transform(X_train)
```

Learns mean/std from training set only.

Then:

```python id="y2c8r1"
transform(X_test)
```

Uses same values.

Avoids data leakage.

---

# Viva

> StandardScaler standardizes features for better optimization.

---

# 🔥 SECTION — Logistic Regression

```python id="p4k9d7"
model = LogisticRegression()
model.fit(X_train, y_train)
```

---

# Despite name, used for Classification

Output:

```text id="u6x1q8"
0 or 1
```

---

# Mathematical Idea

Linear score:

```text id="n5m8r3"
z = b0 + b1x1 + b2x2 + b3x3
```

Then sigmoid:

```text id="t7p2c6"
p = 1 / (1 + e^-z)
```

Probability between 0 and 1.

---

# Final Decision

```text id="r1k9m4"
p > 0.5 → class 1
else class 0
```

---

# fit()

Learns best weights.

---

# Viva

> Logistic Regression predicts probability using sigmoid function.

---

# 🔥 Prediction

```python id="q8w2e5"
y_pred = model.predict(X_test)
```

---

# Meaning

Uses unseen X_test and predicts classes.

---

# Example

```text id="d6t3v1"
Age=48 Salary=120000 → 1
```

---

# 🔥 Confusion Matrix

```python id="s9m4k7"
cm = confusion_matrix(y_test, y_pred)
```

---

# Structure

```text id="x3q8w1"
[[TN FP]
 [FN TP]]
```

---

# Meaning

## TN

Actual no, predicted no

## FP

Actual no, predicted yes

## FN

Actual yes, predicted no

## TP

Actual yes, predicted yes

---

# ravel()

```python id="j5d2c9"
TN, FP, FN, TP = cm.ravel()
```

Converts matrix to 4 values.

---

# Example

```text id="w1m6r8"
[[65 3]
 [8 24]]
```

Means:

```text id="z4t7q2"
TN=65
FP=3
FN=8
TP=24
```

---

# 🔥 Metrics

```python id="g2n8v4"
accuracy = accuracy_score(...)
precision = precision_score(...)
recall = recall_score(...)
error_rate = 1 - accuracy
```

---

# Accuracy

```text id="p6x3m9"
(TP+TN)/Total
```

---

# Precision

When model says YES, how often correct?

```text id="r8w5k1"
TP/(TP+FP)
```

---

# Recall

How many real YES found?

```text id="u3q7c6"
TP/(TP+FN)
```

---

# Error Rate

```text id="f9n2d5"
1 - accuracy
```

---

# Viva

> Accuracy gives overall correctness, precision checks positive correctness, recall checks positive detection.

---

# Classification Report

```python id="h7m1r4"
print(classification_report(y_test, y_pred))
```

Shows:

```text id="a4v8p2"
precision
recall
f1-score
support
```

---

# Scatter Plot

```python id="y6k2n9"
plt.scatter(range(len(y_test)), y_test)
plt.scatter(range(len(y_pred)), y_pred)
```

---

# Meaning

Compares actual and predicted labels by row index.

But for binary classification, confusion matrix is more useful.

---

# Common Beginner Mistakes

## ❌ Scaling before split

Wrong.

## ❌ Using fit_transform on test

Wrong.

## ❌ Thinking logistic regression is regression output

Wrong.

## ❌ Using text data directly

Need encoding.

---

# 30-Second Viva Summary

> First I loaded the dataset using pandas, checked shape and missing values, filled null values using mean, encoded gender using LabelEncoder, visualized correlation with heatmap and outliers with boxplot, selected features X and target y, split data into train and test sets, scaled features using StandardScaler, trained Logistic Regression model, predicted results and evaluated using confusion matrix, accuracy, precision and recall.

---

# Most Important Memory Shortcut

```text id="f1q9n6"
X = inputs
y = output

fit = learn
predict = guess

train = practice
test = exam
```

---

# Honest Improvement Suggestion

Your scatter plot is weak for classification. Replace with confusion matrix heatmap for better practical presentation.
