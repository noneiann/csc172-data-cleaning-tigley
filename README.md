# csc172-data-cleaning-tigley

Data Cleaning Assignment for CSC172

# AI Citations

1. Best Approach for skewed age data set grouped by male, and female, and 1st 2nd and third class
   Model: Gemini 2.5
   Prompt: In data Cleaning, what is the best Approach for skewed age data set grouped by gender (male and female), and class (1,2,3)

   Generated Solution:
   The best approach is to handle the skewed age data within each group defined by gender and class, rather than applying a single method to the entire dataset. The specific technique you choose depends on your goal and the machine learning model you plan to use.

   For most scenarios, the most robust starting point is a group-wise log transformation.

   Applying a single cleaning method (like taking the median of all ages) to the entire dataset is a mistake. The age distribution for a male in class 1 is likely very different from a female in class 3. Treating each subgroup independently—for example, (male, class 1), (female, class 2), etc.—preserves the unique characteristics of these groups and leads to a more accurate model.

   How to implement: Group your data by gender and class, then apply the transformation to the age column within each group.

2. Group by syntax
   Model: ChatGPT
   Prompt: How to group by and change values in pandas
   Generated Code:

   1. Group and aggregate (summarize)

   If you want to compute something (like mean, sum, count, etc.) per group:

   df.groupby('column_name')['target_column'].mean()

   Example:

   ```{python}

   df = pd.DataFrame({
   'City': ['Manila', 'Manila', 'Cebu', 'Cebu'],
   'Sales': [100, 200, 150, 300]
   })

   df.groupby('City')['Sales'].sum()
   ```

   ```
   2. Group and assign a new value back

   If you want to modify or replace a column based on group operations, use transform().

   For example, set each value to the group mean:

   df['Sales_mean'] = df.groupby('City')['Sales'].transform('mean')

   3. Modify values conditionally per group

   You can use groupby().apply() to apply a custom function to each group.

   Example: set all Sales below the group’s mean to the mean value.

   def adjust_sales(group):
   mean = group['Sales'].mean()
   group.loc[group['Sales'] < mean, 'Sales'] = mean
   return group

   df = df.groupby('City', group_keys=False).apply(adjust_sales)

   This changes values within each group.

   4. Replace values based on group rank, size, etc.

   Example — add group size:

   df['City_count'] = df.groupby('City')['City'].transform('count')

   Example — replace with group rank:

   df['Sales_rank'] = df.groupby('City')['Sales'].rank()
   ```

```

```

3. Duplicate columns to variable
   Prompt: how to get duplicate column values in panda and input in a variable, just code
   Generated Code:

   ```{python}
   import pandas as pd

   # Example DataFrame
   df = pd.DataFrame({
       'Name': ['Alice', 'Bob', 'Alice', 'Charlie', 'Bob'],
       'Age': [24, 30, 24, 18, 30]
   })

   # Get duplicate values in a column and store in variable
   duplicates = df['Name'][df['Name'].duplicated()].unique().tolist()

   print(duplicates)
   ```

4.
