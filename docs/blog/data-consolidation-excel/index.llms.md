![](pandas.jpg)

# Introduction

In this tutorial, we will demonstrate how to automatically consolidate data from multiple sheets of an Excel file into a single pandas DataFrame. This approach is particularly useful when working with large Excel files that contain several sheets, especially when the number of sheets is unknown or subject to change.

By the end of this tutorial, you’ll be able to:

- Dynamically retrieve all sheet names from an Excel file.

- Read data from each sheet and combine them into a single DataFrame.

- Add a column to the final DataFrame that tracks the source sheet for each row.

We’ll be using Python’s popular libraries—`pandas` for data manipulation and `numpy` for numerical operations.

Once you have the `numpy` and `pandas` libraries installed, start by importing them. We will import `numpy` for numerical operations and `pandas` for data handling.

``` python
import numpy as np
import pandas as pd
```

## Step 1: Loading the Excel File and Retrieving Sheet Names

The first task is to load the Excel file and retrieve all available sheet names. We use the `pd.ExcelFile()` function to load the file, and the `sheet_names` attribute to dynamically retrieve the list of sheets.

``` python
# Load the Excel file
excel_file = pd.ExcelFile('Diamonds.xlsx')

# Retrieve all sheet names
sheet_names = excel_file.sheet_names
```

At this point, `sheet_names` will be a list of all the sheet names in the Excel file. For example,

``` python
sheet_names
```

    ['Fair', 'Good', 'Very Good', 'Premium', 'Ideal']

## Step 2: Reading and Concatenating Data from All Sheets

With the sheet names available, the next step is to read the data from each sheet and concatenate it into a single DataFrame. We use a dictionary comprehension to load each sheet into a DataFrame, and then use the `concat()` function to combine them. We also add a column that records the name of the sheet from which each row originated.

``` python
# Read all sheets and concatenate them into one DataFrame, adding a sheet_name column
df_dict = {sheet: pd.read_excel(excel_file, sheet_name=sheet) for sheet in sheet_names}

data = pd.concat(df.assign(sheet_name=name) for name, df in df_dict.items())
```

This code reads each sheet into a dictionary (`df_dict`), where the keys are sheet names and the values are DataFrames. The `concat()` function then merges all the DataFrames together into a single DataFrame, and the `sheet_name` column ensures that each row has a record of its original sheet.

## Step 3: Inspecting the Combined Data

Once the data has been successfully consolidated, it’s always a good practice to inspect it and verify that everything was combined correctly. The `head()` and `tail()` functions are useful for this.

``` python
# View the first few rows of the data
data.head()
```

|     | carat | color | clarity | depth | table | price | x    | y    | z    | sheet_name |
|-----|-------|-------|---------|-------|-------|-------|------|------|------|------------|
| 0   | 2.00  | I     | SI1     | 65.9  | 60.0  | 13764 | 7.80 | 7.73 | 5.12 | Fair       |
| 1   | 0.70  | H     | SI1     | 65.2  | 58.0  | 2048  | 5.49 | 5.55 | 3.60 | Fair       |
| 2   | 1.51  | E     | SI1     | 58.4  | 70.0  | 11102 | 7.55 | 7.39 | 4.36 | Fair       |
| 3   | 0.70  | D     | SI2     | 65.5  | 57.0  | 1806  | 5.56 | 5.43 | 3.60 | Fair       |
| 4   | 0.35  | F     | VVS1    | 54.6  | 59.0  | 1011  | 4.85 | 4.79 | 2.63 | Fair       |

``` python
# View the last few rows of the data
data.tail()
```

|     | carat | color | clarity | depth | table | price | x    | y    | z    | sheet_name |
|-----|-------|-------|---------|-------|-------|-------|------|------|------|------------|
| 55  | 0.41  | E     | VS1     | 62.3  | 57.0  | 1153  | 4.77 | 4.74 | 2.96 | Ideal      |
| 56  | 0.40  | E     | VS1     | 62.1  | 55.0  | 898   | 4.75 | 4.79 | 2.96 | Ideal      |
| 57  | 0.53  | G     | SI1     | 60.7  | 56.0  | 1371  | 5.22 | 5.27 | 3.18 | Ideal      |
| 58  | 0.45  | I     | VS1     | 62.1  | 55.0  | 825   | 4.90 | 4.92 | 3.05 | Ideal      |
| 59  | 1.03  | G     | SI1     | 61.6  | 55.0  | 5518  | 6.48 | 6.52 | 4.00 | Ideal      |

This allows you to verify that:

1.  Data from all sheets has been loaded.

2.  The `sheet_name` column contains the correct sheet names for each row.

## Step 4: Saving the Consolidated Data

If you want to save the consolidated data for further use, you can export it to a new Excel file or a CSV file using the following code:

#### Saving to an Excel file:

``` python
data.to_excel('Consolidated_Data.xlsx', index=False)
```

#### Saving to a CSV file:

``` python
data.to_csv('Consolidated_Data.csv', index=False)
```

# Conclusion

By following this tutorial, you’ve learned how to automatically load and consolidate data from multiple Excel sheets into a single pandas DataFrame. This approach dynamically retrieves sheet names, making it flexible and scalable. You can now easily manage datasets that are spread across multiple Excel sheets without needing to manually specify the sheet names each time.

### Additional Tips:

If the Excel sheets have varying structures (e.g., different column names), you might need to adjust how the sheets are concatenated, ensuring that data is aligned properly.

``` python
# Read all sheets and concatenate them, aligning columns by name

data = pd.concat([df_dict[sheet] for sheet in sheet_names], ignore_index=True, sort=False)
```

The `sort = False` parameter prevents pandas from automatically sorting the columns alphabetically, preserving their original order.

Back to top
