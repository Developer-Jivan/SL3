# Practical 7: NLP Preprocessing + TF-IDF

## Deep Explanation (Code + Concept Focused)

This code does **two tasks**:

```text id="4lwpvc"
1. Text Preprocessing
2. TF-IDF Representation
```

That matches your practical statement exactly.

---

# FULL FLOW

```text id="q30fse"
Sample Documents
      ↓
Take First Document
      ↓
Lowercase
      ↓
Tokenize
      ↓
POS Tagging
      ↓
Remove Stopwords
      ↓
Stemming
      ↓
Lemmatization
      ↓
All Documents → TF-IDF Matrix
      ↓
Show IDF Values
```

---

# SECTION 1 — IMPORT LIBRARIES

```python id="kz56si"
import nltk
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
```

---

## Concept

We need tools.

### `nltk`

Used for NLP operations:

* tokenize
* POS tagging
* stopwords
* stemming
* lemmatization

### `pandas`

Used to display output in table format.

### `TfidfVectorizer`

Converts text into numeric TF-IDF matrix.

---

---

# SECTION 2 — IMPORT NLP FUNCTIONS

```python id="8ujfqn"
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk import pos_tag
from nltk.stem import PorterStemmer, WordNetLemmatizer
```

---

## Why?

Instead of writing:

```python id="kvvyl9"
nltk.tokenize.word_tokenize()
```

You can directly write:

```python id="kg35kh"
word_tokenize()
```

Cleaner code.

---

# SECTION 3 — DOWNLOAD DATA

```python id="6y1fxh"
nltk.download(...)
```

---

## Why Needed?

NLTK functions need extra files.

| Package                    | Purpose           |
| -------------------------- | ----------------- |
| punkt                      | tokenization      |
| stopwords                  | common words list |
| averaged_perceptron_tagger | POS tagging       |
| wordnet                    | lemmatization     |

---

# SECTION 4 — SAMPLE DOCUMENTS

```python id="jtb48u"
docs = [
 ...
]
```

---

## Concept

This is dataset.

Each string = one document.

```text id="0vqvzv"
docs[0] = first document
docs[1] = second document
```

---

# Why Multiple Documents?

Because TF-IDF needs many documents to compare word importance.

---

# SECTION 5 — TAKE FIRST DOCUMENT

```python id="kww2kw"
text = docs[0].lower()
```

---

## What Happens?

Take first sentence:

```text id="13vn7m"
Natural language processing helps computers understand human language
```

Convert to lowercase:

```text id="c47f9h"
natural language processing helps computers understand human language
```

---

## Why Lowercase?

So these are treated same:

```text id="q0qol1"
Language
language
LANGUAGE
```

---

# TASK 1 — PREPROCESSING

---

# 1️⃣ TOKENIZATION

```python id="v4j7mj"
tokens = word_tokenize(text)
```

---

## Concept

Break sentence into words.

---

## Output

```python id="4s9jaj"
[
'natural',
'language',
'processing',
'helps',
'computers',
'understand',
'human',
'language'
]
```

---

## Why Needed?

Computers process words better than full sentence.

---

# 2️⃣ POS TAGGING

```python id="m1g1wb"
pos_tag(tokens)
```

---

## Concept

Assign grammar role to each word.

---

## Example Output

```python id="x7am9p"
[
('natural','JJ'),
('language','NN'),
('processing','NN'),
('helps','VBZ')
]
```

---

## Meaning

| Tag | Meaning   |
| --- | --------- |
| NN  | Noun      |
| JJ  | Adjective |
| VBZ | Verb      |

---

## Why Useful?

Used in:

* grammar checking
* meaning extraction
* chatbot understanding

---

# 3️⃣ STOPWORD REMOVAL

```python id="a4v6i9"
filtered = [w for w in tokens if w not in stopwords.words('english')]
```

---

## Concept

Remove common low-value words.

Examples:

```text id="fup83t"
is, the, and, of, to
```

---

## Here

Your sentence may already have few/no stopwords.

So output may remain same.

---

## Why Useful?

Removes noise.

---

# Code Logic

For each token:

```text id="j0rcrf"
If word is not in stopword list
    keep it
```

---

# 4️⃣ STEMMING

```python id="7v31yw"
stemmer = PorterStemmer()
stemmed = [stemmer.stem(w) for w in filtered]
```

---

## Concept

Cuts words to rough root form.

---

## Example

| Original   | Stem    |
| ---------- | ------- |
| processing | process |
| helps      | help    |
| computers  | comput  |

---

## Note

Stem may not be proper English word.

---

## Why Use?

Fast text normalization.

---

# 5️⃣ LEMMATIZATION

```python id="j3gxft"
lemmatizer = WordNetLemmatizer()
lemmatized = [lemmatizer.lemmatize(w) for w in filtered]
```

---

## Concept

Converts word to dictionary base form.

---

## Example

| Original  | Lemma    |
| --------- | -------- |
| helps     | help     |
| computers | computer |
| cars      | car      |

---

## Better Than Stemming?

Usually yes.

Because result is meaningful word.

---

# Difference

| Stemming      | Lemmatization   |
| ------------- | --------------- |
| rough cut     | dictionary root |
| faster        | slower          |
| less accurate | more accurate   |

---

# TASK 2 — TF-IDF

---

# Code

```python id="9eg0t9"
vectorizer = TfidfVectorizer()
tfidf = vectorizer.fit_transform(docs)
```

---

# Big Concept

Machine learning needs numbers, not text.

So convert documents into vectors.

---

# `fit_transform()` Means Two Steps

## `fit()`

Learn all unique words from documents.

Example vocabulary:

```text id="r7j6tl"
artificial
computers
data
language
machine
processing
science
```

---

## `transform()`

Convert each document into numeric row.

---

# TF Meaning

## Term Frequency

How often word appears in a document.

Formula:

```text id="apjlwm"
TF = count of word in document / total words in document
```

---

# Example

Document:

```text id="4xy6yl"
language human language
```

Word = language

```text id="jlwm0v"
TF = 2 / 3
```

---

# IDF Meaning

## Inverse Document Frequency

Rare words get more importance.

Formula:

```text id="8jj18v"
IDF = log(Total Documents / Documents containing word)
```

---

# Logic

If word appears everywhere:

```text id="9iqym5"
low importance
```

If word appears in only one document:

```text id="c59j44"
high importance
```

---

# Final Formula

```text id="03b8yc"
TF-IDF = TF × IDF
```

---

# Why Better Than Normal Count?

Simple count says:

```text id="14lhzc"
common word repeated = important
```

But TF-IDF says:

```text id="m6v0f7"
rare + repeated = more meaningful
```

---

# Convert to Table

```python id="h77x9m"
tfidf_df = pd.DataFrame(...)
```

---

## Why?

Readable format.

Each:

* Row = document
* Column = word
* Value = importance score

---

# Example Table

| Doc | language | machine | science |
| --- | -------- | ------- | ------- |
| 1   | 0.62     | 0       | 0       |
| 2   | 0        | 0.71    | 0       |
| 3   | 0        | 0       | 0.77    |

---

# How to Read

Higher value = more important word in that document.

0 = word not present.

---

# IDF Values

```python id="az1ptl"
vectorizer.idf_
```

---

## Meaning

Returns IDF score of every vocabulary word.

Then shown using:

```python id="l6f1ad"
idf_df = pd.DataFrame(...)
```

---

# Example

| Word     | IDF |
| -------- | --- |
| language | 1.5 |
| machine  | 1.9 |
| data     | 1.9 |

---

# Interpretation

`language` appears in more docs → lower IDF

`machine` rarer → higher IDF

---

# Why Pandas Used?

```python id="axhrf0"
pd.DataFrame(...)
```

To display clean tables.

Without pandas, raw arrays look messy.

---

# Important Functions Summary

| Function          | Purpose                |
| ----------------- | ---------------------- |
| lower()           | lowercase              |
| word_tokenize()   | split words            |
| pos_tag()         | grammar tags           |
| stopwords.words() | common words           |
| stem()            | rough root             |
| lemmatize()       | dictionary root        |
| fit_transform()   | learn + convert        |
| toarray()         | sparse → normal matrix |

---

# Viva Questions

## Q1 Why lowercase?

Avoid duplicate forms.

## Q2 Why tokenization first?

Other steps need words.

## Q3 Why remove stopwords?

Reduce unnecessary common words.

## Q4 Why stemming and lemmatization both?

To demonstrate two normalization methods.

## Q5 Why TF-IDF?

Convert text to weighted numeric form.

## Q6 Why show IDF separately?

To explicitly satisfy practical statement.

---

# Beginner Mistakes

## Mistake 1

Using TF-IDF on one document only.

Need multiple docs for meaningful IDF.

## Mistake 2

Thinking stemming gives correct English always.

No.

## Mistake 3

Ignoring lowercase.

---

# Final Summary

```text id="j0h0cq"
Raw Text
↓
Tokenize
↓
POS Tag
↓
Remove Stopwords
↓
Stem / Lemma
↓
TF-IDF Matrix
↓
IDF Scores
↓
ML Ready Data
```

---

# Honest Feedback

This version is much better than earlier long one.

* Short
* Clean
* Exact practical requirements
* Easier to explain in viva

---

# If you want next, I can also give you:

## 🔥 Exact output of this code line by line

## 🔥 How TF-IDF values are mathematically calculated for your documents

## 🔥 20 viva questions with answers for Practical 7
