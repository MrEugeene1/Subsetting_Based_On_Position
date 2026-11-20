<div align="center">

# 📍 Subsetting Data by Position with Pandas

**Learn to pick exactly the rows and columns you want from a DataFrame using where they are, not what they're called.**

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-DataFrame-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-2ea44f)

</div>

---

## 📚 Table of Contents

1. [What You'll Learn](#-what-youll-learn)
2. [Project Structure](#-project-structure)
3. [The Dataset](#-the-dataset)
4. [Getting Started](#-getting-started)
5. [Key Idea: Positions Start at 0](#-key-idea-positions-start-at-0)
6. [Lesson 1: Top and Bottom Rows with `head()` and `tail()`](#1️⃣-top-and-bottom-rows-with-head-and-tail)
7. [Lesson 2: A Range of Rows with Slicing](#2️⃣-a-range-of-rows-with-slicing)
8. [Lesson 3: Specific Rows with `iloc`](#3️⃣-specific-rows-with-iloc)
9. [Lesson 4: Specific Rows and Columns with `iloc`](#4️⃣-specific-rows-and-columns-with-iloc)
10. [Cheat Sheet](#-cheat-sheet)
11. [Common Pitfalls](#️-common-pitfalls)
12. [Practice Exercises](#-practice-exercises)
13. [What's Next](#-whats-next)

---

## 🎯 What You'll Learn

By the end of this notebook you will be able to:

- ✅ Preview a dataset quickly with `head()` and `tail()`
- ✅ Grab a block of consecutive rows using slice notation (`data[10:15]`)
- ✅ Select any rows you like, in any order, using `iloc`
- ✅ Select rows **and** columns together by their position with `iloc`

---

## 🗂 Project Structure

```
Subsetting based on Position/
├── 1. Subsetting- Position Based.ipynb   ← the lesson notebook
├── datasets/
│   └── big_mart_sales.csv                ← the data we explore
└── README.md                             ← you are here
```

---

## 🛒 The Dataset

**Big Mart Sales** holds product and store information from a retail chain: **8,523 rows × 12 columns**.

| Position | Column | Description |
|:---:|---|---|
| 0 | `Item_Identifier` | Unique product ID |
| 1 | `Item_Weight` | Weight of the product |
| 2 | `Item_Fat_Content` | Low Fat / Regular |
| 3 | `Item_Visibility` | Share of display area given to the product |
| 4 | `Item_Type` | Product category (Dairy, Soft Drinks, …) |
| 5 | `Item_MRP` | Maximum retail price |
| 6 | `Outlet_Identifier` | Unique store ID |
| 7 | `Outlet_Establishment_Year` | Year the store opened |
| 8 | `Outlet_Size` | Small / Medium / High |
| 9 | `Outlet_Location_Type` | Tier of city the store is in |
| 10 | `Outlet_Type` | Grocery store or supermarket type |
| 11 | `Item_Outlet_Sales` | Sales of the product at that store |

> 💡 Keep this table handy. When you select columns **by position**, these numbers are what you'll type.

---

## 🚀 Getting Started

**1. Install the requirements**

```bash
pip install pandas jupyter
```

**2. Open the notebook**

```bash
jupyter notebook "1. Subsetting- Position Based.ipynb"
```

Or open it directly in **VS Code** with the Jupyter extension.

**3. Load the data** (the first cells of the notebook do this)

```python
import pandas as pd

data = pd.read_csv('datasets/big_mart_sales.csv')
```

---

## 🔢 Key Idea: Positions Start at 0

Pandas counts the way Python does, starting from **0**:

```
            col 0            col 1        col 2          ...   col 11
          ┌────────────────┬────────────┬──────────────┬─────┬──────────────────┐
 row 0    │ FDA15          │ 9.30       │ Low Fat      │ ... │ 3735.1380        │
 row 1    │ DRC01          │ 5.92       │ Regular      │ ... │  443.4228        │
 row 2    │ FDN15          │ 17.50      │ Low Fat      │ ... │ 2097.2700        │
  ...     │ ...            │ ...        │ ...          │ ... │ ...              │
 row 8522 │ DRG01          │ 14.80      │ Low Fat      │ ... │ ...              │
          └────────────────┴────────────┴──────────────┴─────┴──────────────────┘
```

So the **1st** row is position `0`, the **2nd** row is position `1`, and the **last** of 8,523 rows is position `8522`.

---

## 1️⃣ Top and Bottom Rows with `head()` and `tail()`

The quickest way to see what your data looks like.

| Method | What it returns |
|---|---|
| `data.head()` | First **5** rows (the default) |
| `data.head(n)` | First **n** rows |
| `data.tail()` | Last **5** rows |
| `data.tail(n)` | Last **n** rows |

```python
data.head(2)   # first 2 rows → positions 0, 1
data.head()    # first 5 rows → positions 0 to 4
data.tail(2)   # last 2 rows  → positions 8521, 8522
```

> 🧠 **Why it matters:** run `head()` right after loading any file. It shows you the column names, the kind of values in each, and whether the file loaded the way you expected.

---

## 2️⃣ A Range of Rows with Slicing

Use square brackets with a `start:stop` slice to grab consecutive rows.

```python
data[10:15]   # rows at positions 10, 11, 12, 13, 14
```

```
start ─┐        ┌─ stop (NOT included)
       ▼        ▼
      [10 : 15]   →   10, 11, 12, 13, 14   (5 rows)
```

> ⚠️ The **stop** value is **excluded**. `data[10:15]` gives 5 rows, not 6.
> A quick way to remember it: **number of rows = stop − start**.

More slicing patterns:

```python
data[:3]      # first 3 rows      (same as head(3))
data[-3:]     # last 3 rows       (same as tail(3))
data[0:10:2]  # every 2nd row from 0 to 9 → 0, 2, 4, 6, 8
```

---

## 3️⃣ Specific Rows with `iloc`

`iloc` stands for **i**nteger **loc**ation. Pass it a **list** of row positions and you get back exactly those rows, **in the order you listed them**.

```python
data.iloc[[1, 5, 2, 4, 6, 14]]
```

Result (index column shown first):

```
    Item_Identifier  Item_Weight  Item_Fat_Content  ...
1   DRC01            5.920        Regular
5   FDP36            10.395       Regular
2   FDN15            17.500       Low Fat
4   NCD19            8.930        Low Fat
6   FDO10            13.650       Regular
14  FDF32            16.350       Low Fat
```

> 🔍 Notice the **double brackets** `[[ ... ]]`: the outer pair belongs to `iloc`, the inner pair is a Python list of positions.

---

## 4️⃣ Specific Rows and Columns with `iloc`

Give `iloc` **two** things separated by a comma: **rows first, then columns**.

```
data.iloc[ rows , columns ]
              ▲        ▲
              │        └── which columns (by position)
              └─────────── which rows    (by position)
```

```python
data.iloc[[1, 4, 5, 2], [1, 3, 5]]
```

Columns `1, 3, 5` are `Item_Weight`, `Item_Visibility` and `Item_MRP`:

```
   Item_Weight  Item_Visibility  Item_MRP
1        5.920         0.019278   48.2692
4        8.930         0.000000   53.8614
5       10.395         0.000000   51.4008
2       17.500         0.016760  141.6180
```

You can also mix lists and slices:

```python
data.iloc[0:5, 0:3]      # first 5 rows, first 3 columns
data.iloc[:, [0, 11]]    # every row, only the ID and the sales column
data.iloc[3, 5]          # a single value: row 3, column 5 (Item_MRP)
```

> 💡 A lone `:` means "everything" along that axis.

---

## 📋 Cheat Sheet

| Goal | Code |
|---|---|
| First 5 rows | `data.head()` |
| First n rows | `data.head(n)` |
| Last n rows | `data.tail(n)` |
| Rows from a to b−1 | `data[a:b]` |
| Specific rows | `data.iloc[[r1, r2, r3]]` |
| Specific rows and columns | `data.iloc[[r1, r2], [c1, c2]]` |
| Block of rows and columns | `data.iloc[r_start:r_stop, c_start:c_stop]` |
| All rows, some columns | `data.iloc[:, [c1, c2]]` |
| One single cell | `data.iloc[r, c]` |
| Last row | `data.iloc[-1]` |

---

## ⚠️ Common Pitfalls

| Mistake | What happens | Fix |
|---|---|---|
| `data.iloc[1, 5, 2]` | Error: too many indexers | Wrap positions in a list: `data.iloc[[1, 5, 2]]` |
| Expecting `data[10:15]` to include row 15 | You get rows 10–14 | Slices stop **before** the stop value |
| `data.iloc[[1, 2], ['Item_MRP']]` | Error: `iloc` only takes integers | Use the position (`5`), or use `loc` for names |
| Thinking the 1st row is position 1 | You're off by one | Positions start at **0** |
| Asking for position `8523` | `IndexError` | The last valid position is `len(data) - 1` |

---

## 🏋️ Practice Exercises

Try each one in the notebook before opening the answer.

**1.** Show the first 10 rows of the data.
<details><summary>Answer</summary>

```python
data.head(10)
```
</details>

**2.** Select rows 100 to 104 (5 rows).
<details><summary>Answer</summary>

```python
data[100:105]
```
</details>

**3.** Select rows 0, 50 and 100, in that order.
<details><summary>Answer</summary>

```python
data.iloc[[0, 50, 100]]
```
</details>

**4.** For rows 7, 3 and 9, show only `Item_Type` and `Item_Outlet_Sales`.
<details><summary>Answer</summary>

```python
data.iloc[[7, 3, 9], [4, 11]]
```
</details>

**5.** Get the value of `Item_MRP` in the very last row.
<details><summary>Answer</summary>

```python
data.iloc[-1, 5]
```
</details>

---

## 🧭 What's Next

Position-based selection is one half of the story. Next, learn **label-based subsetting** with `loc`, where you select rows and columns by their **names** and by **conditions** (for example, "all Dairy products priced above 100").

| | `iloc` | `loc` |
|---|---|---|
| Selects by | Integer position | Label / name |
| Example | `data.iloc[[0, 1], [5]]` | `data.loc[[0, 1], ['Item_MRP']]` |
| Slice end | Excluded | **Included** |

---

<div align="center">

**Happy learning! 🐼📊**

*If this helped you, give the repo a ⭐*

</div>
