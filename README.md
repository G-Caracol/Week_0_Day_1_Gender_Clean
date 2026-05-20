# Week_0_Day_1_Gender_Clean
This is in alignment with the Week 0 Day 1 activity given in Carisurg that is mean to be submitted via this github link

The *google notebook* link is here [google notebook](https://colab.research.google.com/drive/1PdjiMRNP4aUmm0u8gkjtJfdxjP7-wp4f?usp=sharing)

## Table of Contents

1. [What We Used](https://www.google.com/search?q=%23what-we-used)
2. [Step 1: Scope Out the Data](https://www.google.com/search?q=%23step-1-scope-out-the-data)
3. [Step 2: Spotting the Mess in the Gender Column](https://www.google.com/search?q=%23step-2-spotting-the-mess-in-the-gender-column)
4. [Step 3: Crafting the Cleanup Pipeline](https://www.google.com/search?q=%23step-3-crafting-the-cleanup-pipeline)
5. [Step 4: Swapping the Data Without Messing Up the Order](https://www.google.com/search?q=%23step-4-swapping-the-data-without-messing-up-the-order)
6. [Step 5: Final Check](https://www.google.com/search?q=%23step-5-final-check)

---

## What We Used

* **`pandas`**: Used to read our CSV file into memory as a DataFrame so we could perform operations on it.
* **`sys`**: Used as a quick utility to check the running Python version.
* **`google.colab.drive`**: Used to mount Google Drive so the notebook could access the stored CSV file.

```python
import sys
import pandas as pd
from google.colab import drive

drive.mount('/content/drive')

```

---

## Step 1: Scope Out the Data

We loaded the dataset and checked its shape, column names, and a quick preview of the rows to see what we were dealing with. The data contains 2,205 rows and 11 columns, including metrics like `GCS`, `SBP`, `DBP`, and `RR`.

```python
df_raw = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/clinical_dataset.csv')

print(df_raw.shape)
print(df_raw.columns)
df_raw.head(10)

```

---

## Step 2: Spotting the Mess in the Gender Column

We checked the unique values in the `Gender` column and found a messy mix of different casing styles and data types.

```python
print(df_raw["Gender"].unique())
# Output: ['0' 'Male' 'Female' 'FEMALE' '1' 'MALE']

```

---

## Step 3: Crafting the Cleanup Pipeline

We created a mapping dictionary to convert all the variations into clean binary flags: 0 for female and 1 for male. Then, we chained together a pandas line to stringify, lowercase, strip whitespace, and map the values into a temporary column.

```python
gender_map = {
    '0': 0,
    '1': 1,
    'male': 1,
    'female': 0
}

df_raw["Clean_Gender"] = df_raw["Gender"].astype(str).str.strip().str.lower().map(gender_map)

```

---

## Step 4: Swapping the Data Without Messing Up the Order

We assigned the values of the new column back into the original `Gender` column so it wouldn't lose its position in the table. After that, we safely dropped the temporary helper column.

```python
df_raw["Gender"] = df_raw["Clean_Gender"]
df_raw = df_raw.drop(columns=["Clean_Gender"])

```

---

## Step 5: Final Check

We printed out the first few rows of the DataFrame one last time to make sure everything looked correct. The values are now completely uniform across all 2,205 records.

```python
df_raw.head(5)

```
