# Aim of This Practical

Implement Gaussian Naïve Bayes on the Iris Dataset with:

* outlier detection
* outlier visualization
* classification
* confusion matrix metrics

---

# What This Dataset Contains

Each row represents a flower.

The model predicts flower species:

```text id="56g4v8"
Setosa
Versicolor
Virginica
```

using flower measurements.

---

# Dataset Columns

| Column       | Type                 | Meaning        |
| ------------ | -------------------- | -------------- |
| sepal.length | Continuous Numerical | Sepal length   |
| sepal.width  | Continuous Numerical | Sepal width    |
| petal.length | Continuous Numerical | Petal length   |
| petal.width  | Continuous Numerical | Petal width    |
| variety      | Categorical Target   | Flower species |

---

# Complete Workflow

```text id="4s1bzx"
Load Dataset
      ↓
Check Missing Values
      ↓
Detect Outliers
      ↓
Visualize Outliers
      ↓
Handle Outliers
      ↓
Separate Features & Target
      ↓
Encode Target Labels
      ↓
Train-Test Split
      ↓
Gaussian Naive Bayes
      ↓
Prediction
      ↓
Confusion Matrix & Metrics
```

---

# Step-by-Step Explanation

# Step 1: Import Libraries

```python id="qh2upf"
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

---

# Why These Libraries?

## Pandas

```python id="qk6g8r"
import pandas as pd
```

Used for:

* reading dataset
* handling rows and columns

---

## NumPy

```python id="z8sq8h"
import numpy as np
```

Used for:

* numerical operations
* outlier capping using `np.where()`

---

## Matplotlib

```python id="9f92m4"
import matplotlib.pyplot as plt
```

Used for:

* boxplots
* graph visualization

---

# Machine Learning Imports

```python id="k8v0ta"
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.naive_bayes import GaussianNB
```

---

# train_test_split

Splits dataset into:

* training data
* testing data

---

# LabelEncoder

Converts text labels into numbers.

Example:

```text id="k0nhf8"
Setosa → 0
Versicolor → 1
Virginica → 2
```

---

# GaussianNB

Naïve Bayes model for continuous numerical features.

Used because iris features are decimal numeric values.

---

# Metrics Imports

```python id="24w5mc"
from sklearn.metrics import ...
```

Used for:

* confusion matrix
* accuracy
* precision
* recall

---

# Step 2: Load Dataset

```python id="1p2mjlwm"
df = pd.read_csv("iris.csv")
```

---

# What Happens?

Reads CSV file into dataframe.

---

# Step 3: Display First 5 Rows

```python id="ysrnyh"
print(df.head())
```

---

# Purpose

Used to:

* verify dataset loaded correctly
* inspect column names
* inspect sample data

---

# Step 4: Dataset Information

```python id="km81r9"
print(df.info())
```

---

# What It Shows

* total rows
* total columns
* datatype
* non-null count

---

# Expected Datatypes

| Column       | Datatype |
| ------------ | -------- |
| sepal.length | float    |
| sepal.width  | float    |
| petal.length | float    |
| petal.width  | float    |
| variety      | object   |

---

# Step 5: Missing Values

```python id="3c6nfd"
print(df.isnull().sum())
```

---

# Purpose

Checks missing values column-wise.

---

# Why Important?

Machine learning algorithms cannot properly handle empty values.

---

# Your Dataset Result

Usually:

```text id="yz6txj"
0 missing values
```

So:

* no imputation needed

---

# Step 6: Numerical Columns

```python id="9znd8f"
numerical_cols = [...]
```

---

# Why This List Created?

Outlier detection should apply ONLY to:

* continuous numerical columns

NOT:

* categorical columns
* target column

---

# Why variety Excluded?

Because:

```text id="6q6sod"
variety
```

is categorical target data.

Outlier detection on categories is meaningless.

---

# Step 7: IQR Outlier Detection

```python id="br5nrw"
Q1 = df[col].quantile(0.25)
Q3 = df[col].quantile(0.75)
```

---

# What Are Q1 and Q3?

| Term | Meaning         |
| ---- | --------------- |
| Q1   | 25th percentile |
| Q3   | 75th percentile |

---

# Interquartile Range

```python id="sp6ybe"
IQR = Q3 - Q1
```

---

# Meaning

Measures spread of middle 50% data.

---

# Outlier Limits

```python id="r33xmq"
lower_limit = Q1 - 1.5 * IQR
upper_limit = Q3 + 1.5 * IQR
```

---

# Logic

Values:

* below lower limit
* above upper limit

are considered statistical outliers.

---

# Finding Outliers

```python id="8vzzwp"
outliers = df[
    (df[col] < lower_limit) |
    (df[col] > upper_limit)
]
```

---

# What This Does

Filters rows containing extreme values.

---

# Output

```python id="wsv14u"
print("Number of Outliers:", len(outliers))
```

Shows outlier count for each column.

---

# Step 8: Boxplot Visualization

```python id="gddj85"
df.boxplot(column=numerical_cols)
```

---

# What Is Boxplot?

Graph used to visualize:

* spread
* median
* outliers

---

# Boxplot Structure

```text id="lpjlwm"
Extreme ─ whisker ─ box ─ whisker ─ Extreme
```

Points outside whiskers:

* possible outliers

---

# Why Visualization Important?

Helps visually inspect:

* whether outliers are severe
* whether treatment needed

---

# Step 9: Outlier Handling

```python id="s3hn0v"
np.where()
```

---

# What Is Happening?

Extreme values are capped.

---

# Example

```text id="q72a0f"
Upper limit = 7.5

Original value = 8.2

After capping:
7.5
```

---

# Why Capping Used?

Instead of deleting rows:

* preserves dataset size
* reduces effect of extreme values

---

# Important Note

For iris dataset:

* these outliers are often valid observations

So in real professional preprocessing:

* handling may not even be necessary

Your earlier observation was correct.

---

# Step 10: Features and Target

```python id="0dqlsx"
X = df.drop("variety", axis=1)
y = df["variety"]
```

---

# Features (X)

Input columns:

* sepal.length
* sepal.width
* petal.length
* petal.width

---

# Target (y)

Output column:

* variety

---

# Step 11: Encode Target Labels

```python id="u24q07"
le = LabelEncoder()
y = le.fit_transform(y)
```

---

# Why Needed?

Machine learning algorithms work better with numeric labels.

---

# Label Conversion

```text id="g2bqtq"
Setosa → 0
Versicolor → 1
Virginica → 2
```

---

# Why ONLY Target Encoded?

Because:

* feature columns already numeric
* only variety contains text

---

# Step 12: Train-Test Split

```python id="tcrg94"
train_test_split(...)
```

---

# What Happens?

Dataset split into:

```text id="a5xjlwm"
80% → Training
20% → Testing
```

---

# Why?

Model must be tested on unseen data.

Otherwise:

* model only memorizes dataset

---

# random_state=42

Ensures:

* same split every run
* reproducible results

---

# Step 13: Create Model

```python id="8t5lq0"
model = GaussianNB()
```

---

# Why GaussianNB?

Naïve Bayes variants:

| Variant       | Used For                  |
| ------------- | ------------------------- |
| GaussianNB    | Continuous numerical data |
| MultinomialNB | Count/text data           |
| BernoulliNB   | Binary data               |

---

# Your Dataset Type

Feature columns:

* decimal continuous values

Therefore:

```python id="gmxyc4"
GaussianNB()
```

is correct.

---

# Step 14: Train Model

```python id="4ghuwq"
model.fit(X_train, y_train)
```

---

# What Happens Internally?

Model learns patterns like:

```text id="jlwmm4"
Small petals → likely Setosa
Large petals → likely Virginica
```

---

# Step 15: Prediction

```python id="jlwmj6"
y_pred = model.predict(X_test)
```

---

# Meaning

Predicts flower species for unseen test data.

---

# Step 16: Confusion Matrix

```python id="k9t0ns"
cm = confusion_matrix(y_test, y_pred)
```

---

# What Is Confusion Matrix?

Comparison between:

* actual classes
* predicted classes

---

# Structure

```text id="o7d4rn"
              Predicted
            0   1   2

Actual 0
Actual 1
Actual 2
```

Diagonal values:

* correct predictions

Off-diagonal:

* wrong predictions

---

# Step 17: TP FP TN FN

```python id="jlwmw3"
TP = cm[i, i]
```

---

# Meanings

## TP — True Positive

Correctly predicted class.

---

## FP — False Positive

Incorrectly predicted as class.

---

## FN — False Negative

Actual class missed.

---

## TN — True Negative

Correctly rejected other classes.

---

# ASCII Diagram

```text id="i9jjlwm"
                    Actual

                YES         NO

Pred YES        TP          FP

Pred NO         FN          TN
```

---

# Step 18: Accuracy

```python id="85mzuh"
accuracy_score(y_test, y_pred)
```

---

# Formula

```text id="jlwmad"
Accuracy =
Correct Predictions / Total Predictions
```

---

# Step 19: Error Rate

```python id="bjlwmr"
1 - accuracy
```

---

# Meaning

Percentage of wrong predictions.

---

# Step 20: Precision

```python id="jlwmly"
precision_score(..., average='weighted')
```

---

# Formula

```text id="jlwmn2"
TP / (TP + FP)
```

---

# Meaning

Out of predicted positives:

* how many were correct?

---

# Step 21: Recall

```python id="jlwm01"
recall_score(..., average='weighted')
```

---

# Formula

```text id="jlwmmo"
TP / (TP + FN)
```

---

# Meaning

Out of actual positives:

* how many correctly detected?

---

# Final Workflow Diagram

```text id="jlwm0f"
Iris Dataset
      ↓
Missing Value Check
      ↓
Outlier Detection
      ↓
Outlier Visualization
      ↓
Outlier Handling
      ↓
Feature & Target Separation
      ↓
Label Encoding
      ↓
Train-Test Split
      ↓
Gaussian Naive Bayes
      ↓
Prediction
      ↓
Confusion Matrix
      ↓
Accuracy / Precision / Recall
```

---

# Important Viva Questions

## Why GaussianNB used?

Because dataset features are continuous numerical values.

---

## Why Encode Target Column?

Machine learning algorithms work efficiently with numerical labels.

---

## Why Outlier Detection Only on Feature Columns?

Because outlier detection is meaningful only for numerical continuous data.

---

## Why Use IQR Method?

Because it is simple and effective for detecting statistical outliers.

---

## Why Use Capping Instead of Removing Rows?

To reduce outlier influence without reducing dataset size.

---

## Why Might Outlier Handling Be Optional Here?

Because iris dataset is already a clean benchmark dataset and many extreme values are genuine observations rather than errors.
