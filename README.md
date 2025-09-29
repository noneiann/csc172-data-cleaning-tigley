# Assignment 0: Data Cleaning with AI

## Student Information

- **Name:** Rey Iann V. Tigley
- **Course Year:** BSCS 4
- **Date:** 2025-09-29

## Dataset

- **Source:** [Kaggle – Titanic: Machine Learning from Disaster](https://www.kaggle.com/c/titanic)
- **Name:** Titanic Dataset

## Issues Found

- **Missing values:** `Age`, `Embarked`, and `Cabin` columns contained missing data.
- **Outliers:** Outliers were identified in `Fare` and `Age`, but intentionally retained.

## Cleaning Steps

1. **Missing Values**

   - **Age:** Filled missing values using the **median Age per group** defined by `Pclass` and `Sex`.
     ```python
     age_groups = df.groupby(['Pclass', 'Sex'])['Age']
     group_medians = age_groups.transform('median')
     df['Age'] = df['Age'].fillna(group_medians)
     ```
   - **Embarked:** Filled missing values with the **mode** (most common port).
     ```python
     df["Embarked"] = df['Embarked'].fillna(df["Embarked"].mode()[0])
     ```
   - **Cabin:** Filled missing values with `"U"` to represent “Unknown.”
     ```python
     df["Cabin"] = df["Cabin"].fillna("U")
     ```

2. **Duplicates**
   DISCLAIMER: there are actually zero duplicate rows and zero duplicate names with same ticket but I still put them there just to be sure.

   - Removed **exact duplicate rows**.
     ```python
     df = df.drop_duplicates()
     ```
   - Checked for **duplicate names** and removed records with **duplicate tickets under the same name**.
     ```python
     duplicate_names = df[df.duplicated(subset=['Name'], keep=False)]
     duplicate_tickets = duplicate_names[duplicate_names.duplicated(subset=['Ticket'], keep=False)]
     df = df.drop(duplicate_tickets.index)
     ```

3. **Standardization**

   - Converted text columns to consistent formatting:
     ```python
     df['Sex'] = df['Sex'].str.lower()
     df['Embarked'] = df['Embarked'].str.upper()
     df['Name'] = df['Name'].str.title()
     ```

4. **Outliers**

   - Checked `Fare` and `Age` using the IQR method but **not to remove** any outliers since they are real world values.

5. **Export**
   - Saved the cleaned dataset for later analysis.
     ```python
     df.to_csv("../data/cleaned_dataset.csv", index=False)
     ```

## AI Prompts Used

1. **Best approach for skewed age data**

   - **Model:** Gemini 2.5
   - **Prompt:**
     > “In data cleaning, what is the best approach for skewed age dataset grouped by gender (male and female), and class (1, 2, 3)?”
   - **Proposed Solution:** Handle missing `Age` values by grouping the data by `Pclass` and `Sex`, then applying a median or log transformation per group.

2. **Group by syntax**

   - **Model:** ChatGPT
   - **Prompt:**
     > “How to group by and change values in pandas”
   - **Generated Code:**
     ```python
     df['Sales_mean'] = df.groupby('City')['Sales'].transform('mean')
     ```

3. **Duplicate column values**
   - **Model:** ChatGPT
   - **Prompt:**
     > “How to get duplicate column values in pandas and input in a variable (just code)”
   - **Generated Code:**
     ```python
     duplicates = df['Name'][df['Name'].duplicated()].unique().tolist()
     ```
4. **README.MD Formatting**
   - **Model:** ChatGPT
   - **Prompt:**
     > “Format my uploaded readme.md properly”
   - **Generated Code:**
     This formatted Readme.md

## Results

- **Rows before cleaning:** 891
- **Rows after cleaning:** 891
- **Outcome:** Cleaned Titanic dataset with missing values filled, duplicates handled, standardized formats applied, and meaningful outliers retained for analysis.
