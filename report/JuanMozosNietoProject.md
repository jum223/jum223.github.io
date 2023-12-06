---
layout: wide_default
---

# Juan Mozos Nieto - FIN336 - Data Analytics Project

### Necessary Imports


```python
import pandas as pd
import warnings
from statsmodels.formula.api import ols as sm_ols
from statsmodels.iolib.summary2 import summary_col
```

#### Reading in the data


```python
with warnings.catch_warnings():
    warnings.simplefilter("ignore")
    df = pd.read_csv("2023Fall_Freddie_Assignment_Data.csv")
```

#### Changing display options for better output visibility


```python
pd.set_option("display.max_columns", None)
```

#### Initial look at the data


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>oyear</th>
      <th>v1</th>
      <th>v2</th>
      <th>v3</th>
      <th>v4</th>
      <th>v5</th>
      <th>v6</th>
      <th>v7</th>
      <th>v8</th>
      <th>v9</th>
      <th>v10</th>
      <th>v11</th>
      <th>v12</th>
      <th>v13</th>
      <th>v14</th>
      <th>v15</th>
      <th>v16</th>
      <th>v17</th>
      <th>v18</th>
      <th>v19</th>
      <th>v20</th>
      <th>v21</th>
      <th>v22</th>
      <th>v23</th>
      <th>v24</th>
      <th>v25</th>
      <th>v26</th>
      <th>v27</th>
      <th>v28</th>
      <th>v29</th>
      <th>v30</th>
      <th>v31</th>
      <th>v32</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1999</td>
      <td>654</td>
      <td>199906</td>
      <td>Y</td>
      <td>202905</td>
      <td>23104.0</td>
      <td>30</td>
      <td>1</td>
      <td>P</td>
      <td>95</td>
      <td>45</td>
      <td>85000</td>
      <td>95</td>
      <td>7.000</td>
      <td>R</td>
      <td>N</td>
      <td>FRM</td>
      <td>TX</td>
      <td>SF</td>
      <td>76100</td>
      <td>F99Q20302845</td>
      <td>P</td>
      <td>360</td>
      <td>2</td>
      <td>Other sellers</td>
      <td>Other servicers</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>N</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1999</td>
      <td>768</td>
      <td>199907</td>
      <td>N</td>
      <td>201906</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>P</td>
      <td>72</td>
      <td>33</td>
      <td>154000</td>
      <td>72</td>
      <td>7.125</td>
      <td>R</td>
      <td>N</td>
      <td>FRM</td>
      <td>MN</td>
      <td>SF</td>
      <td>55700</td>
      <td>F99Q20165628</td>
      <td>C</td>
      <td>240</td>
      <td>1</td>
      <td>NORWEST MORTGAGE, INC.</td>
      <td>WELLS FARGO HOME MORTGAGE, INC.</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>N</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1999</td>
      <td>760</td>
      <td>200002</td>
      <td>N</td>
      <td>201501</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>P</td>
      <td>80</td>
      <td>41</td>
      <td>240000</td>
      <td>80</td>
      <td>7.625</td>
      <td>T</td>
      <td>N</td>
      <td>FRM</td>
      <td>MN</td>
      <td>SF</td>
      <td>55300</td>
      <td>F99Q40076986</td>
      <td>C</td>
      <td>180</td>
      <td>2</td>
      <td>FIFTH THIRD BANK</td>
      <td>Other servicers</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>N</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1999</td>
      <td>756</td>
      <td>200002</td>
      <td>N</td>
      <td>203001</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>P</td>
      <td>23</td>
      <td>21</td>
      <td>20000</td>
      <td>23</td>
      <td>8.250</td>
      <td>T</td>
      <td>N</td>
      <td>FRM</td>
      <td>MD</td>
      <td>SF</td>
      <td>21600</td>
      <td>F99Q40230828</td>
      <td>P</td>
      <td>360</td>
      <td>1</td>
      <td>NORWEST MORTGAGE, INC.</td>
      <td>WELLS FARGO BANK, N.A.</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>N</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1999</td>
      <td>738</td>
      <td>199911</td>
      <td>N</td>
      <td>201410</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
      <td>S</td>
      <td>80</td>
      <td>28</td>
      <td>71000</td>
      <td>80</td>
      <td>7.875</td>
      <td>R</td>
      <td>N</td>
      <td>FRM</td>
      <td>MN</td>
      <td>SF</td>
      <td>55800</td>
      <td>F99Q30198593</td>
      <td>P</td>
      <td>180</td>
      <td>2</td>
      <td>NORWEST MORTGAGE, INC.</td>
      <td>WELLS FARGO HOME MORTGAGE, INC.</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>N</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### Initial look at column names


```python
df.columns
```




    Index(['oyear', 'v1', 'v2', 'v3', 'v4', 'v5', 'v6', 'v7', 'v8', 'v9', 'v10',
           'v11', 'v12', 'v13', 'v14', 'v15', 'v16', 'v17', 'v18', 'v19', 'v20',
           'v21', 'v22', 'v23', 'v24', 'v25', 'v26', 'v27', 'v28', 'v29', 'v30',
           'v31', 'v32'],
          dtype='object')



#### Renaming the Columns


```python
new_column_names = {
    "v1": "CREDIT SCORE",
    "v2": "FIRST PAYMENT DATE",
    "v3": "FIRST TIME HOMEBUYER FLAG",
    "v4": "MATURITY DATE",
    "v5": "MSA",
    "v6": "MORTGAGE INSURANCE PERCENTAGE (MI %)",
    "v7": "NUMBER OF UNITS",
    "v8": "OCCUPANCY STATUS",
    "v9": "ORIGINAL COMBINED LOAN-TO-VALUE (CLTV)",
    "v10": "ORIGINAL DEBT-TO-INCOME (DTI) RATIO",
    "v11": "ORIGINAL UPB",
    "v12": "ORIGINAL LOAN-TO-VALUE (LTV)",
    "v13": "ORIGINAL INTEREST RATE",
    "v14": "CHANNEL",
    "v15": "PREPAYMENT PENALTY MORTGAGE (PPM) FLAG",
    "v16": "AMORTIZATION TYPE",
    "v17": "PROPERTY STATE",
    "v18": "PROPERTY TYPE",
    "v19": "POSTAL CODE",
    "v20": "LOAN SEQUENCE NUMBER",
    "v21": "LOAN PURPOSE",
    "v22": "ORIGINAL LOAN TERM",
    "v23": "NUMBER OF BORROWERS",
    "v24": "SELLER NAME",
    "v25": "SERVICER NAME",
    "v26": "SUPER CONFORMING FLAG",
    "v27": "PRE-RELIEF REFINANCE LOAN SEQUENCE NUMBER",
    "v28": "PROGRAM INDICATOR",
    "v29": "RELIEF REFINANCE INDICATOR",
    "v30": "PROPERTY VALUATION METHOD",
    "v31": "INTEREST ONLY INDICATOR (I/O INDICATOR)",
    "v32": "MI CANCELLATION INDICATOR",
}

df.rename(columns=new_column_names, inplace=True)
```

#### Checking change in column names


```python
df.columns
```




    Index(['oyear', 'CREDIT SCORE', 'FIRST PAYMENT DATE',
           'FIRST TIME HOMEBUYER FLAG', 'MATURITY DATE', 'MSA',
           'MORTGAGE INSURANCE PERCENTAGE (MI %)', 'NUMBER OF UNITS',
           'OCCUPANCY STATUS', 'ORIGINAL COMBINED LOAN-TO-VALUE (CLTV)',
           'ORIGINAL DEBT-TO-INCOME (DTI) RATIO', 'ORIGINAL UPB',
           'ORIGINAL LOAN-TO-VALUE (LTV)', 'ORIGINAL INTEREST RATE', 'CHANNEL',
           'PREPAYMENT PENALTY MORTGAGE (PPM) FLAG', 'AMORTIZATION TYPE',
           'PROPERTY STATE', 'PROPERTY TYPE', 'POSTAL CODE',
           'LOAN SEQUENCE NUMBER', 'LOAN PURPOSE', 'ORIGINAL LOAN TERM',
           'NUMBER OF BORROWERS', 'SELLER NAME', 'SERVICER NAME',
           'SUPER CONFORMING FLAG', 'PRE-RELIEF REFINANCE LOAN SEQUENCE NUMBER',
           'PROGRAM INDICATOR', 'RELIEF REFINANCE INDICATOR',
           'PROPERTY VALUATION METHOD', 'INTEREST ONLY INDICATOR (I/O INDICATOR)',
           'MI CANCELLATION INDICATOR'],
          dtype='object')



### Data Cleaning

#### Replacing null value indicators with actual null values


```python
df["CREDIT SCORE"] = df["CREDIT SCORE"].replace(9999, pd.NA)
df["FIRST TIME HOMEBUYER FLAG"] = df["FIRST TIME HOMEBUYER FLAG"].replace("9",pd.NA)
df["MORTGAGE INSURANCE PERCENTAGE (MI %)"] = df["MORTGAGE INSURANCE PERCENTAGE (MI %)"].replace(999, pd.NA)
df["NUMBER OF UNITS"] = df["NUMBER OF UNITS"].replace(99, pd.NA)
df["OCCUPANCY STATUS"] = df["OCCUPANCY STATUS"].replace("9",pd.NA)
df["ORIGINAL COMBINED LOAN-TO-VALUE (CLTV)"] = df["ORIGINAL COMBINED LOAN-TO-VALUE (CLTV)"].replace(999, pd.NA)
df["ORIGINAL DEBT-TO-INCOME (DTI) RATIO"] = df["ORIGINAL DEBT-TO-INCOME (DTI) RATIO"].replace(999, pd.NA)
df["ORIGINAL LOAN-TO-VALUE (LTV)"] = df["ORIGINAL LOAN-TO-VALUE (LTV)"].replace(999, pd.NA)
df["CHANNEL"] = df["CHANNEL"].replace("9", pd.NA)
df["PROPERTY TYPE"] = df["PROPERTY TYPE"].replace("99", pd.NA)
df["LOAN PURPOSE"] = df["LOAN PURPOSE"].replace("9", pd.NA)
df["NUMBER OF BORROWERS"] = df["NUMBER OF BORROWERS"].replace(99, pd.NA)
df["PROGRAM INDICATOR"] = df["PROGRAM INDICATOR"].replace(9, pd.NA)
df["PROPERTY VALUATION METHOD"] = df["PROPERTY VALUATION METHOD"].replace(9, pd.NA)
df["MI CANCELLATION INDICATOR"] = df["MI CANCELLATION INDICATOR"].replace("7", pd.NA)
```

#### Creating a copy of the original df, so that the original df is not altered


```python
df1 = df.copy()
```

## Question 1

#### Creating dummy variables


```python
df1["First"] = df1["FIRST TIME HOMEBUYER FLAG"].map({"Y": 1, "N": 0})
df1["Penalty"] = df1["PREPAYMENT PENALTY MORTGAGE (PPM) FLAG"].map({"Y" : 1, "N": 0})
```

#### Changing Column Names


```python
new_names = {"ORIGINAL INTEREST RATE" : "Rate",
             "ORIGINAL UPB" : "UPB",
             "ORIGINAL LOAN TERM" : "Term",
             "ORIGINAL LOAN-TO-VALUE (LTV)" : "LTV",
             "ORIGINAL DEBT-TO-INCOME (DTI) RATIO" : "DTI",
             "CREDIT SCORE" : "FICO",
             "NUMBER OF BORROWERS" : "Borrowers",
}
df1.rename(columns=new_names, inplace=True)
```

#### Converting Column Types (Python Treats LTV, DTI, FICO and Borrowers as Object Types)


```python
numeric_columns = ["LTV", "DTI", "FICO", "Borrowers"]
for column in numeric_columns:
    df1[column] = pd.to_numeric(df1[column], errors="coerce")
```

#### Creating the Table


```python
# Selecting the variables
variables = ["Rate",
             "UPB",
             "Term",
             "LTV",
             "DTI",
             "FICO",
             "Borrowers",
             "First",
             "Penalty"]

# Creating summary table
summary_table = df1[variables].describe(percentiles=[])

# Renaming the 50% percentile column to median
summary_table = summary_table.rename(index={"50%": "median"})

summary_table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rate</th>
      <th>UPB</th>
      <th>Term</th>
      <th>LTV</th>
      <th>DTI</th>
      <th>FICO</th>
      <th>Borrowers</th>
      <th>First</th>
      <th>Penalty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>72750.000000</td>
      <td>7.275000e+04</td>
      <td>72750.000000</td>
      <td>72749.000000</td>
      <td>66972.000000</td>
      <td>72613.000000</td>
      <td>72735.000000</td>
      <td>72698.00000</td>
      <td>72750.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5.130097</td>
      <td>2.035716e+05</td>
      <td>314.051395</td>
      <td>71.683776</td>
      <td>33.842606</td>
      <td>740.687824</td>
      <td>1.557682</td>
      <td>0.12955</td>
      <td>0.001306</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.423189</td>
      <td>1.218134e+05</td>
      <td>77.145301</td>
      <td>18.859577</td>
      <td>10.915987</td>
      <td>52.472380</td>
      <td>0.501678</td>
      <td>0.33581</td>
      <td>0.036113</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.750000</td>
      <td>8.000000e+03</td>
      <td>60.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>300.000000</td>
      <td>1.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>median</th>
      <td>4.989500</td>
      <td>1.750000e+05</td>
      <td>360.000000</td>
      <td>75.000000</td>
      <td>34.000000</td>
      <td>751.000000</td>
      <td>2.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>13.700000</td>
      <td>1.200000e+06</td>
      <td>480.000000</td>
      <td>312.000000</td>
      <td>65.000000</td>
      <td>839.000000</td>
      <td>4.000000</td>
      <td>1.00000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Polishing the table


```python
formatting_rules = {
    "Rate": '{:.1f}',
    "UPB": '{:,.0f}',
    "Term": '{:,.0f}',
    "LTV": '{:.1f}',
    "DTI": '{:.1f}',
    "FICO": '{:,.0f}',
    "Borrowers": '{:.1f}',
    "First": '{:.2f}',
    "Penalty": '{:.3f}',
}

formatted_summary_table = summary_table.style.format(formatting_rules)
```

### Final Question 1 Table


```python
formatted_summary_table
```




<style type="text/css">
</style>
<table id="T_eff44">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_eff44_level0_col0" class="col_heading level0 col0" >Rate</th>
      <th id="T_eff44_level0_col1" class="col_heading level0 col1" >UPB</th>
      <th id="T_eff44_level0_col2" class="col_heading level0 col2" >Term</th>
      <th id="T_eff44_level0_col3" class="col_heading level0 col3" >LTV</th>
      <th id="T_eff44_level0_col4" class="col_heading level0 col4" >DTI</th>
      <th id="T_eff44_level0_col5" class="col_heading level0 col5" >FICO</th>
      <th id="T_eff44_level0_col6" class="col_heading level0 col6" >Borrowers</th>
      <th id="T_eff44_level0_col7" class="col_heading level0 col7" >First</th>
      <th id="T_eff44_level0_col8" class="col_heading level0 col8" >Penalty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_eff44_level0_row0" class="row_heading level0 row0" >count</th>
      <td id="T_eff44_row0_col0" class="data row0 col0" >72750.0</td>
      <td id="T_eff44_row0_col1" class="data row0 col1" >72,750</td>
      <td id="T_eff44_row0_col2" class="data row0 col2" >72,750</td>
      <td id="T_eff44_row0_col3" class="data row0 col3" >72749.0</td>
      <td id="T_eff44_row0_col4" class="data row0 col4" >66972.0</td>
      <td id="T_eff44_row0_col5" class="data row0 col5" >72,613</td>
      <td id="T_eff44_row0_col6" class="data row0 col6" >72735.0</td>
      <td id="T_eff44_row0_col7" class="data row0 col7" >72698.00</td>
      <td id="T_eff44_row0_col8" class="data row0 col8" >72750.000</td>
    </tr>
    <tr>
      <th id="T_eff44_level0_row1" class="row_heading level0 row1" >mean</th>
      <td id="T_eff44_row1_col0" class="data row1 col0" >5.1</td>
      <td id="T_eff44_row1_col1" class="data row1 col1" >203,572</td>
      <td id="T_eff44_row1_col2" class="data row1 col2" >314</td>
      <td id="T_eff44_row1_col3" class="data row1 col3" >71.7</td>
      <td id="T_eff44_row1_col4" class="data row1 col4" >33.8</td>
      <td id="T_eff44_row1_col5" class="data row1 col5" >741</td>
      <td id="T_eff44_row1_col6" class="data row1 col6" >1.6</td>
      <td id="T_eff44_row1_col7" class="data row1 col7" >0.13</td>
      <td id="T_eff44_row1_col8" class="data row1 col8" >0.001</td>
    </tr>
    <tr>
      <th id="T_eff44_level0_row2" class="row_heading level0 row2" >std</th>
      <td id="T_eff44_row2_col0" class="data row2 col0" >1.4</td>
      <td id="T_eff44_row2_col1" class="data row2 col1" >121,813</td>
      <td id="T_eff44_row2_col2" class="data row2 col2" >77</td>
      <td id="T_eff44_row2_col3" class="data row2 col3" >18.9</td>
      <td id="T_eff44_row2_col4" class="data row2 col4" >10.9</td>
      <td id="T_eff44_row2_col5" class="data row2 col5" >52</td>
      <td id="T_eff44_row2_col6" class="data row2 col6" >0.5</td>
      <td id="T_eff44_row2_col7" class="data row2 col7" >0.34</td>
      <td id="T_eff44_row2_col8" class="data row2 col8" >0.036</td>
    </tr>
    <tr>
      <th id="T_eff44_level0_row3" class="row_heading level0 row3" >min</th>
      <td id="T_eff44_row3_col0" class="data row3 col0" >1.8</td>
      <td id="T_eff44_row3_col1" class="data row3 col1" >8,000</td>
      <td id="T_eff44_row3_col2" class="data row3 col2" >60</td>
      <td id="T_eff44_row3_col3" class="data row3 col3" >3.0</td>
      <td id="T_eff44_row3_col4" class="data row3 col4" >1.0</td>
      <td id="T_eff44_row3_col5" class="data row3 col5" >300</td>
      <td id="T_eff44_row3_col6" class="data row3 col6" >1.0</td>
      <td id="T_eff44_row3_col7" class="data row3 col7" >0.00</td>
      <td id="T_eff44_row3_col8" class="data row3 col8" >0.000</td>
    </tr>
    <tr>
      <th id="T_eff44_level0_row4" class="row_heading level0 row4" >median</th>
      <td id="T_eff44_row4_col0" class="data row4 col0" >5.0</td>
      <td id="T_eff44_row4_col1" class="data row4 col1" >175,000</td>
      <td id="T_eff44_row4_col2" class="data row4 col2" >360</td>
      <td id="T_eff44_row4_col3" class="data row4 col3" >75.0</td>
      <td id="T_eff44_row4_col4" class="data row4 col4" >34.0</td>
      <td id="T_eff44_row4_col5" class="data row4 col5" >751</td>
      <td id="T_eff44_row4_col6" class="data row4 col6" >2.0</td>
      <td id="T_eff44_row4_col7" class="data row4 col7" >0.00</td>
      <td id="T_eff44_row4_col8" class="data row4 col8" >0.000</td>
    </tr>
    <tr>
      <th id="T_eff44_level0_row5" class="row_heading level0 row5" >max</th>
      <td id="T_eff44_row5_col0" class="data row5 col0" >13.7</td>
      <td id="T_eff44_row5_col1" class="data row5 col1" >1,200,000</td>
      <td id="T_eff44_row5_col2" class="data row5 col2" >480</td>
      <td id="T_eff44_row5_col3" class="data row5 col3" >312.0</td>
      <td id="T_eff44_row5_col4" class="data row5 col4" >65.0</td>
      <td id="T_eff44_row5_col5" class="data row5 col5" >839</td>
      <td id="T_eff44_row5_col6" class="data row5 col6" >4.0</td>
      <td id="T_eff44_row5_col7" class="data row5 col7" >1.00</td>
      <td id="T_eff44_row5_col8" class="data row5 col8" >1.000</td>
    </tr>
  </tbody>
</table>




## Question 2

#### Creating Correlation Matrix


```python
variables = ["Rate",
             "UPB",
             "Term",
             "LTV",
             "DTI",
             "FICO",
             "Borrowers",
             "First",
             "Penalty"]

corr_matrix = df1[variables].corr().style.format("{:.2f}")
corr_matrix
```




<style type="text/css">
</style>
<table id="T_1c5c6">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_1c5c6_level0_col0" class="col_heading level0 col0" >Rate</th>
      <th id="T_1c5c6_level0_col1" class="col_heading level0 col1" >UPB</th>
      <th id="T_1c5c6_level0_col2" class="col_heading level0 col2" >Term</th>
      <th id="T_1c5c6_level0_col3" class="col_heading level0 col3" >LTV</th>
      <th id="T_1c5c6_level0_col4" class="col_heading level0 col4" >DTI</th>
      <th id="T_1c5c6_level0_col5" class="col_heading level0 col5" >FICO</th>
      <th id="T_1c5c6_level0_col6" class="col_heading level0 col6" >Borrowers</th>
      <th id="T_1c5c6_level0_col7" class="col_heading level0 col7" >First</th>
      <th id="T_1c5c6_level0_col8" class="col_heading level0 col8" >Penalty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_1c5c6_level0_row0" class="row_heading level0 row0" >Rate</th>
      <td id="T_1c5c6_row0_col0" class="data row0 col0" >1.00</td>
      <td id="T_1c5c6_row0_col1" class="data row0 col1" >-0.32</td>
      <td id="T_1c5c6_row0_col2" class="data row0 col2" >0.18</td>
      <td id="T_1c5c6_row0_col3" class="data row0 col3" >0.05</td>
      <td id="T_1c5c6_row0_col4" class="data row0 col4" >0.05</td>
      <td id="T_1c5c6_row0_col5" class="data row0 col5" >-0.30</td>
      <td id="T_1c5c6_row0_col6" class="data row0 col6" >0.04</td>
      <td id="T_1c5c6_row0_col7" class="data row0 col7" >0.00</td>
      <td id="T_1c5c6_row0_col8" class="data row0 col8" >0.04</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row1" class="row_heading level0 row1" >UPB</th>
      <td id="T_1c5c6_row1_col0" class="data row1 col0" >-0.32</td>
      <td id="T_1c5c6_row1_col1" class="data row1 col1" >1.00</td>
      <td id="T_1c5c6_row1_col2" class="data row1 col2" >0.21</td>
      <td id="T_1c5c6_row1_col3" class="data row1 col3" >0.13</td>
      <td id="T_1c5c6_row1_col4" class="data row1 col4" >0.12</td>
      <td id="T_1c5c6_row1_col5" class="data row1 col5" >0.13</td>
      <td id="T_1c5c6_row1_col6" class="data row1 col6" >0.09</td>
      <td id="T_1c5c6_row1_col7" class="data row1 col7" >0.06</td>
      <td id="T_1c5c6_row1_col8" class="data row1 col8" >-0.03</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row2" class="row_heading level0 row2" >Term</th>
      <td id="T_1c5c6_row2_col0" class="data row2 col0" >0.18</td>
      <td id="T_1c5c6_row2_col1" class="data row2 col1" >0.21</td>
      <td id="T_1c5c6_row2_col2" class="data row2 col2" >1.00</td>
      <td id="T_1c5c6_row2_col3" class="data row2 col3" >0.30</td>
      <td id="T_1c5c6_row2_col4" class="data row2 col4" >0.16</td>
      <td id="T_1c5c6_row2_col5" class="data row2 col5" >-0.05</td>
      <td id="T_1c5c6_row2_col6" class="data row2 col6" >-0.08</td>
      <td id="T_1c5c6_row2_col7" class="data row2 col7" >0.17</td>
      <td id="T_1c5c6_row2_col8" class="data row2 col8" >-0.03</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row3" class="row_heading level0 row3" >LTV</th>
      <td id="T_1c5c6_row3_col0" class="data row3 col0" >0.05</td>
      <td id="T_1c5c6_row3_col1" class="data row3 col1" >0.13</td>
      <td id="T_1c5c6_row3_col2" class="data row3 col2" >0.30</td>
      <td id="T_1c5c6_row3_col3" class="data row3 col3" >1.00</td>
      <td id="T_1c5c6_row3_col4" class="data row3 col4" >0.15</td>
      <td id="T_1c5c6_row3_col5" class="data row3 col5" >-0.13</td>
      <td id="T_1c5c6_row3_col6" class="data row3 col6" >-0.07</td>
      <td id="T_1c5c6_row3_col7" class="data row3 col7" >0.26</td>
      <td id="T_1c5c6_row3_col8" class="data row3 col8" >-0.02</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row4" class="row_heading level0 row4" >DTI</th>
      <td id="T_1c5c6_row4_col0" class="data row4 col0" >0.05</td>
      <td id="T_1c5c6_row4_col1" class="data row4 col1" >0.12</td>
      <td id="T_1c5c6_row4_col2" class="data row4 col2" >0.16</td>
      <td id="T_1c5c6_row4_col3" class="data row4 col3" >0.15</td>
      <td id="T_1c5c6_row4_col4" class="data row4 col4" >1.00</td>
      <td id="T_1c5c6_row4_col5" class="data row4 col5" >-0.16</td>
      <td id="T_1c5c6_row4_col6" class="data row4 col6" >-0.11</td>
      <td id="T_1c5c6_row4_col7" class="data row4 col7" >0.04</td>
      <td id="T_1c5c6_row4_col8" class="data row4 col8" >-0.02</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row5" class="row_heading level0 row5" >FICO</th>
      <td id="T_1c5c6_row5_col0" class="data row5 col0" >-0.30</td>
      <td id="T_1c5c6_row5_col1" class="data row5 col1" >0.13</td>
      <td id="T_1c5c6_row5_col2" class="data row5 col2" >-0.05</td>
      <td id="T_1c5c6_row5_col3" class="data row5 col3" >-0.13</td>
      <td id="T_1c5c6_row5_col4" class="data row5 col4" >-0.16</td>
      <td id="T_1c5c6_row5_col5" class="data row5 col5" >1.00</td>
      <td id="T_1c5c6_row5_col6" class="data row5 col6" >-0.03</td>
      <td id="T_1c5c6_row5_col7" class="data row5 col7" >-0.01</td>
      <td id="T_1c5c6_row5_col8" class="data row5 col8" >-0.01</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row6" class="row_heading level0 row6" >Borrowers</th>
      <td id="T_1c5c6_row6_col0" class="data row6 col0" >0.04</td>
      <td id="T_1c5c6_row6_col1" class="data row6 col1" >0.09</td>
      <td id="T_1c5c6_row6_col2" class="data row6 col2" >-0.08</td>
      <td id="T_1c5c6_row6_col3" class="data row6 col3" >-0.07</td>
      <td id="T_1c5c6_row6_col4" class="data row6 col4" >-0.11</td>
      <td id="T_1c5c6_row6_col5" class="data row6 col5" >-0.03</td>
      <td id="T_1c5c6_row6_col6" class="data row6 col6" >1.00</td>
      <td id="T_1c5c6_row6_col7" class="data row6 col7" >-0.09</td>
      <td id="T_1c5c6_row6_col8" class="data row6 col8" >0.00</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row7" class="row_heading level0 row7" >First</th>
      <td id="T_1c5c6_row7_col0" class="data row7 col0" >0.00</td>
      <td id="T_1c5c6_row7_col1" class="data row7 col1" >0.06</td>
      <td id="T_1c5c6_row7_col2" class="data row7 col2" >0.17</td>
      <td id="T_1c5c6_row7_col3" class="data row7 col3" >0.26</td>
      <td id="T_1c5c6_row7_col4" class="data row7 col4" >0.04</td>
      <td id="T_1c5c6_row7_col5" class="data row7 col5" >-0.01</td>
      <td id="T_1c5c6_row7_col6" class="data row7 col6" >-0.09</td>
      <td id="T_1c5c6_row7_col7" class="data row7 col7" >1.00</td>
      <td id="T_1c5c6_row7_col8" class="data row7 col8" >-0.01</td>
    </tr>
    <tr>
      <th id="T_1c5c6_level0_row8" class="row_heading level0 row8" >Penalty</th>
      <td id="T_1c5c6_row8_col0" class="data row8 col0" >0.04</td>
      <td id="T_1c5c6_row8_col1" class="data row8 col1" >-0.03</td>
      <td id="T_1c5c6_row8_col2" class="data row8 col2" >-0.03</td>
      <td id="T_1c5c6_row8_col3" class="data row8 col3" >-0.02</td>
      <td id="T_1c5c6_row8_col4" class="data row8 col4" >-0.02</td>
      <td id="T_1c5c6_row8_col5" class="data row8 col5" >-0.01</td>
      <td id="T_1c5c6_row8_col6" class="data row8 col6" >0.00</td>
      <td id="T_1c5c6_row8_col7" class="data row8 col7" >-0.01</td>
      <td id="T_1c5c6_row8_col8" class="data row8 col8" >1.00</td>
    </tr>
  </tbody>
</table>




## Question 3

#### Creating dummy variables (Necessary to specify the int type because if not they will default to boolean)


```python
df1["Purchase"] = (df1["LOAN PURPOSE"] == "P").astype(int)
df1["< 680"] = (df1["FICO"] < 680).astype(int)
df1["Single"] = (df1["Borrowers"] == 1).astype(int)
```

#### Creating subset of the data to include only necessary columns


```python
variables = ["oyear", "Rate", "Purchase", "UPB", "LTV", "DTI", "FICO", "< 680", "Single"]
df_subset = df1[variables]
```


```python
grouped_means = df_subset.groupby("oyear").mean()

#Creating the row with the general averages for every variable 
overall_mean = df_subset.mean()

#Adding the overall means row to the table
grouped_means.loc["Overall Average"] = overall_mean

#Printing the table
grouped_means
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rate</th>
      <th>Purchase</th>
      <th>UPB</th>
      <th>LTV</th>
      <th>DTI</th>
      <th>FICO</th>
      <th>&lt; 680</th>
      <th>Single</th>
    </tr>
    <tr>
      <th>oyear</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1999</th>
      <td>7.337683</td>
      <td>0.486000</td>
      <td>113821.333333</td>
      <td>71.449667</td>
      <td>31.742957</td>
      <td>715.204637</td>
      <td>0.247333</td>
      <td>0.346000</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>8.122338</td>
      <td>0.691667</td>
      <td>124045.000000</td>
      <td>72.887667</td>
      <td>33.594336</td>
      <td>716.345270</td>
      <td>0.253333</td>
      <td>0.384333</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>6.877263</td>
      <td>0.302667</td>
      <td>135673.333333</td>
      <td>70.973333</td>
      <td>31.750000</td>
      <td>720.714334</td>
      <td>0.221333</td>
      <td>0.341667</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>6.392651</td>
      <td>0.258333</td>
      <td>143730.333333</td>
      <td>69.056019</td>
      <td>31.899796</td>
      <td>722.652862</td>
      <td>0.215333</td>
      <td>0.334667</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>5.507776</td>
      <td>0.178333</td>
      <td>145634.333333</td>
      <td>66.001333</td>
      <td>30.800136</td>
      <td>730.778593</td>
      <td>0.172000</td>
      <td>0.343333</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>5.667718</td>
      <td>0.360667</td>
      <td>154214.333333</td>
      <td>68.930667</td>
      <td>33.350746</td>
      <td>722.479118</td>
      <td>0.231333</td>
      <td>0.374667</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>5.812673</td>
      <td>0.376000</td>
      <td>170954.666667</td>
      <td>69.568667</td>
      <td>35.384536</td>
      <td>723.030030</td>
      <td>0.238000</td>
      <td>0.417333</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>6.391042</td>
      <td>0.473000</td>
      <td>176869.333333</td>
      <td>69.732667</td>
      <td>36.521397</td>
      <td>722.490824</td>
      <td>0.245667</td>
      <td>0.432667</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>6.366955</td>
      <td>0.453000</td>
      <td>183082.333333</td>
      <td>71.422000</td>
      <td>36.643906</td>
      <td>726.013009</td>
      <td>0.232333</td>
      <td>0.465667</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>6.032981</td>
      <td>0.421333</td>
      <td>205754.333333</td>
      <td>70.190333</td>
      <td>36.079208</td>
      <td>743.551667</td>
      <td>0.125667</td>
      <td>0.473667</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>4.985427</td>
      <td>0.213333</td>
      <td>215246.000000</td>
      <td>66.443333</td>
      <td>31.847752</td>
      <td>760.279333</td>
      <td>0.047667</td>
      <td>0.388667</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>4.716799</td>
      <td>0.208333</td>
      <td>208135.666667</td>
      <td>68.954000</td>
      <td>31.924307</td>
      <td>759.773333</td>
      <td>0.059000</td>
      <td>0.404333</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>4.426142</td>
      <td>0.220000</td>
      <td>205856.000000</td>
      <td>69.086667</td>
      <td>31.828558</td>
      <td>760.425667</td>
      <td>0.056333</td>
      <td>0.390333</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>3.744633</td>
      <td>0.169667</td>
      <td>205508.000000</td>
      <td>76.430667</td>
      <td>31.146231</td>
      <td>758.083000</td>
      <td>0.069000</td>
      <td>0.421667</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>3.927846</td>
      <td>0.280000</td>
      <td>202030.333333</td>
      <td>75.462000</td>
      <td>32.149458</td>
      <td>751.729910</td>
      <td>0.093333</td>
      <td>0.448667</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>4.308486</td>
      <td>0.498333</td>
      <td>208097.666667</td>
      <td>74.337000</td>
      <td>33.705002</td>
      <td>745.990333</td>
      <td>0.101333</td>
      <td>0.490667</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>3.981578</td>
      <td>0.431667</td>
      <td>226880.666667</td>
      <td>73.478000</td>
      <td>33.502922</td>
      <td>750.394667</td>
      <td>0.089667</td>
      <td>0.479333</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>3.789911</td>
      <td>0.450333</td>
      <td>236025.666667</td>
      <td>73.257667</td>
      <td>33.798460</td>
      <td>748.457667</td>
      <td>0.087000</td>
      <td>0.495000</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>4.188570</td>
      <td>0.564000</td>
      <td>229910.333333</td>
      <td>75.075667</td>
      <td>34.910597</td>
      <td>745.468489</td>
      <td>0.097667</td>
      <td>0.525000</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>4.745135</td>
      <td>0.671333</td>
      <td>229418.000000</td>
      <td>75.840000</td>
      <td>35.770812</td>
      <td>746.669113</td>
      <td>0.095667</td>
      <td>0.544667</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>4.244280</td>
      <td>0.566667</td>
      <td>255544.333333</td>
      <td>76.193333</td>
      <td>35.344678</td>
      <td>750.195667</td>
      <td>0.078667</td>
      <td>0.537333</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>3.193679</td>
      <td>0.294333</td>
      <td>290616.333333</td>
      <td>71.143000</td>
      <td>33.276092</td>
      <td>758.419000</td>
      <td>0.048333</td>
      <td>0.502333</td>
    </tr>
    <tr>
      <th>2021</th>
      <td>2.971172</td>
      <td>0.353333</td>
      <td>291863.333333</td>
      <td>69.328667</td>
      <td>34.100667</td>
      <td>750.557853</td>
      <td>0.081333</td>
      <td>0.540333</td>
    </tr>
    <tr>
      <th>2022</th>
      <td>5.089418</td>
      <td>0.671000</td>
      <td>302796.333333</td>
      <td>73.870333</td>
      <td>37.204667</td>
      <td>743.823608</td>
      <td>0.100667</td>
      <td>0.543333</td>
    </tr>
    <tr>
      <th>2023</th>
      <td>6.330771</td>
      <td>0.825333</td>
      <td>299610.666667</td>
      <td>76.872000</td>
      <td>38.368000</td>
      <td>748.901333</td>
      <td>0.076000</td>
      <td>0.617333</td>
    </tr>
    <tr>
      <th>Overall Average</th>
      <td>5.130097</td>
      <td>0.404110</td>
      <td>203571.573883</td>
      <td>71.683776</td>
      <td>33.842606</td>
      <td>740.687824</td>
      <td>0.136371</td>
      <td>0.444536</td>
    </tr>
  </tbody>
</table>
</div>



#### Polishing the Table


```python
formatting_rules = {
    "Rate": '{:.1f}',
    "Purchase": '{:.2f}',
    "UPB": '{:,.0f}',
    "LTV": '{:.1f}',
    "DTI": '{:.1f}',
    "FICO": '{:,.0f}',
    "< 680": '{:.2f}',
    "Single": '{:.2f}',
}

formatted_grouped_means = grouped_means.style.format(formatting_rules)
```

### Final Question 3 Table


```python
formatted_grouped_means
```




<style type="text/css">
</style>
<table id="T_a6940">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_a6940_level0_col0" class="col_heading level0 col0" >Rate</th>
      <th id="T_a6940_level0_col1" class="col_heading level0 col1" >Purchase</th>
      <th id="T_a6940_level0_col2" class="col_heading level0 col2" >UPB</th>
      <th id="T_a6940_level0_col3" class="col_heading level0 col3" >LTV</th>
      <th id="T_a6940_level0_col4" class="col_heading level0 col4" >DTI</th>
      <th id="T_a6940_level0_col5" class="col_heading level0 col5" >FICO</th>
      <th id="T_a6940_level0_col6" class="col_heading level0 col6" >< 680</th>
      <th id="T_a6940_level0_col7" class="col_heading level0 col7" >Single</th>
    </tr>
    <tr>
      <th class="index_name level0" >oyear</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
      <th class="blank col5" >&nbsp;</th>
      <th class="blank col6" >&nbsp;</th>
      <th class="blank col7" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_a6940_level0_row0" class="row_heading level0 row0" >1999</th>
      <td id="T_a6940_row0_col0" class="data row0 col0" >7.3</td>
      <td id="T_a6940_row0_col1" class="data row0 col1" >0.49</td>
      <td id="T_a6940_row0_col2" class="data row0 col2" >113,821</td>
      <td id="T_a6940_row0_col3" class="data row0 col3" >71.4</td>
      <td id="T_a6940_row0_col4" class="data row0 col4" >31.7</td>
      <td id="T_a6940_row0_col5" class="data row0 col5" >715</td>
      <td id="T_a6940_row0_col6" class="data row0 col6" >0.25</td>
      <td id="T_a6940_row0_col7" class="data row0 col7" >0.35</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row1" class="row_heading level0 row1" >2000</th>
      <td id="T_a6940_row1_col0" class="data row1 col0" >8.1</td>
      <td id="T_a6940_row1_col1" class="data row1 col1" >0.69</td>
      <td id="T_a6940_row1_col2" class="data row1 col2" >124,045</td>
      <td id="T_a6940_row1_col3" class="data row1 col3" >72.9</td>
      <td id="T_a6940_row1_col4" class="data row1 col4" >33.6</td>
      <td id="T_a6940_row1_col5" class="data row1 col5" >716</td>
      <td id="T_a6940_row1_col6" class="data row1 col6" >0.25</td>
      <td id="T_a6940_row1_col7" class="data row1 col7" >0.38</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row2" class="row_heading level0 row2" >2001</th>
      <td id="T_a6940_row2_col0" class="data row2 col0" >6.9</td>
      <td id="T_a6940_row2_col1" class="data row2 col1" >0.30</td>
      <td id="T_a6940_row2_col2" class="data row2 col2" >135,673</td>
      <td id="T_a6940_row2_col3" class="data row2 col3" >71.0</td>
      <td id="T_a6940_row2_col4" class="data row2 col4" >31.8</td>
      <td id="T_a6940_row2_col5" class="data row2 col5" >721</td>
      <td id="T_a6940_row2_col6" class="data row2 col6" >0.22</td>
      <td id="T_a6940_row2_col7" class="data row2 col7" >0.34</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row3" class="row_heading level0 row3" >2002</th>
      <td id="T_a6940_row3_col0" class="data row3 col0" >6.4</td>
      <td id="T_a6940_row3_col1" class="data row3 col1" >0.26</td>
      <td id="T_a6940_row3_col2" class="data row3 col2" >143,730</td>
      <td id="T_a6940_row3_col3" class="data row3 col3" >69.1</td>
      <td id="T_a6940_row3_col4" class="data row3 col4" >31.9</td>
      <td id="T_a6940_row3_col5" class="data row3 col5" >723</td>
      <td id="T_a6940_row3_col6" class="data row3 col6" >0.22</td>
      <td id="T_a6940_row3_col7" class="data row3 col7" >0.33</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row4" class="row_heading level0 row4" >2003</th>
      <td id="T_a6940_row4_col0" class="data row4 col0" >5.5</td>
      <td id="T_a6940_row4_col1" class="data row4 col1" >0.18</td>
      <td id="T_a6940_row4_col2" class="data row4 col2" >145,634</td>
      <td id="T_a6940_row4_col3" class="data row4 col3" >66.0</td>
      <td id="T_a6940_row4_col4" class="data row4 col4" >30.8</td>
      <td id="T_a6940_row4_col5" class="data row4 col5" >731</td>
      <td id="T_a6940_row4_col6" class="data row4 col6" >0.17</td>
      <td id="T_a6940_row4_col7" class="data row4 col7" >0.34</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row5" class="row_heading level0 row5" >2004</th>
      <td id="T_a6940_row5_col0" class="data row5 col0" >5.7</td>
      <td id="T_a6940_row5_col1" class="data row5 col1" >0.36</td>
      <td id="T_a6940_row5_col2" class="data row5 col2" >154,214</td>
      <td id="T_a6940_row5_col3" class="data row5 col3" >68.9</td>
      <td id="T_a6940_row5_col4" class="data row5 col4" >33.4</td>
      <td id="T_a6940_row5_col5" class="data row5 col5" >722</td>
      <td id="T_a6940_row5_col6" class="data row5 col6" >0.23</td>
      <td id="T_a6940_row5_col7" class="data row5 col7" >0.37</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row6" class="row_heading level0 row6" >2005</th>
      <td id="T_a6940_row6_col0" class="data row6 col0" >5.8</td>
      <td id="T_a6940_row6_col1" class="data row6 col1" >0.38</td>
      <td id="T_a6940_row6_col2" class="data row6 col2" >170,955</td>
      <td id="T_a6940_row6_col3" class="data row6 col3" >69.6</td>
      <td id="T_a6940_row6_col4" class="data row6 col4" >35.4</td>
      <td id="T_a6940_row6_col5" class="data row6 col5" >723</td>
      <td id="T_a6940_row6_col6" class="data row6 col6" >0.24</td>
      <td id="T_a6940_row6_col7" class="data row6 col7" >0.42</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row7" class="row_heading level0 row7" >2006</th>
      <td id="T_a6940_row7_col0" class="data row7 col0" >6.4</td>
      <td id="T_a6940_row7_col1" class="data row7 col1" >0.47</td>
      <td id="T_a6940_row7_col2" class="data row7 col2" >176,869</td>
      <td id="T_a6940_row7_col3" class="data row7 col3" >69.7</td>
      <td id="T_a6940_row7_col4" class="data row7 col4" >36.5</td>
      <td id="T_a6940_row7_col5" class="data row7 col5" >722</td>
      <td id="T_a6940_row7_col6" class="data row7 col6" >0.25</td>
      <td id="T_a6940_row7_col7" class="data row7 col7" >0.43</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row8" class="row_heading level0 row8" >2007</th>
      <td id="T_a6940_row8_col0" class="data row8 col0" >6.4</td>
      <td id="T_a6940_row8_col1" class="data row8 col1" >0.45</td>
      <td id="T_a6940_row8_col2" class="data row8 col2" >183,082</td>
      <td id="T_a6940_row8_col3" class="data row8 col3" >71.4</td>
      <td id="T_a6940_row8_col4" class="data row8 col4" >36.6</td>
      <td id="T_a6940_row8_col5" class="data row8 col5" >726</td>
      <td id="T_a6940_row8_col6" class="data row8 col6" >0.23</td>
      <td id="T_a6940_row8_col7" class="data row8 col7" >0.47</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row9" class="row_heading level0 row9" >2008</th>
      <td id="T_a6940_row9_col0" class="data row9 col0" >6.0</td>
      <td id="T_a6940_row9_col1" class="data row9 col1" >0.42</td>
      <td id="T_a6940_row9_col2" class="data row9 col2" >205,754</td>
      <td id="T_a6940_row9_col3" class="data row9 col3" >70.2</td>
      <td id="T_a6940_row9_col4" class="data row9 col4" >36.1</td>
      <td id="T_a6940_row9_col5" class="data row9 col5" >744</td>
      <td id="T_a6940_row9_col6" class="data row9 col6" >0.13</td>
      <td id="T_a6940_row9_col7" class="data row9 col7" >0.47</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row10" class="row_heading level0 row10" >2009</th>
      <td id="T_a6940_row10_col0" class="data row10 col0" >5.0</td>
      <td id="T_a6940_row10_col1" class="data row10 col1" >0.21</td>
      <td id="T_a6940_row10_col2" class="data row10 col2" >215,246</td>
      <td id="T_a6940_row10_col3" class="data row10 col3" >66.4</td>
      <td id="T_a6940_row10_col4" class="data row10 col4" >31.8</td>
      <td id="T_a6940_row10_col5" class="data row10 col5" >760</td>
      <td id="T_a6940_row10_col6" class="data row10 col6" >0.05</td>
      <td id="T_a6940_row10_col7" class="data row10 col7" >0.39</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row11" class="row_heading level0 row11" >2010</th>
      <td id="T_a6940_row11_col0" class="data row11 col0" >4.7</td>
      <td id="T_a6940_row11_col1" class="data row11 col1" >0.21</td>
      <td id="T_a6940_row11_col2" class="data row11 col2" >208,136</td>
      <td id="T_a6940_row11_col3" class="data row11 col3" >69.0</td>
      <td id="T_a6940_row11_col4" class="data row11 col4" >31.9</td>
      <td id="T_a6940_row11_col5" class="data row11 col5" >760</td>
      <td id="T_a6940_row11_col6" class="data row11 col6" >0.06</td>
      <td id="T_a6940_row11_col7" class="data row11 col7" >0.40</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row12" class="row_heading level0 row12" >2011</th>
      <td id="T_a6940_row12_col0" class="data row12 col0" >4.4</td>
      <td id="T_a6940_row12_col1" class="data row12 col1" >0.22</td>
      <td id="T_a6940_row12_col2" class="data row12 col2" >205,856</td>
      <td id="T_a6940_row12_col3" class="data row12 col3" >69.1</td>
      <td id="T_a6940_row12_col4" class="data row12 col4" >31.8</td>
      <td id="T_a6940_row12_col5" class="data row12 col5" >760</td>
      <td id="T_a6940_row12_col6" class="data row12 col6" >0.06</td>
      <td id="T_a6940_row12_col7" class="data row12 col7" >0.39</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row13" class="row_heading level0 row13" >2012</th>
      <td id="T_a6940_row13_col0" class="data row13 col0" >3.7</td>
      <td id="T_a6940_row13_col1" class="data row13 col1" >0.17</td>
      <td id="T_a6940_row13_col2" class="data row13 col2" >205,508</td>
      <td id="T_a6940_row13_col3" class="data row13 col3" >76.4</td>
      <td id="T_a6940_row13_col4" class="data row13 col4" >31.1</td>
      <td id="T_a6940_row13_col5" class="data row13 col5" >758</td>
      <td id="T_a6940_row13_col6" class="data row13 col6" >0.07</td>
      <td id="T_a6940_row13_col7" class="data row13 col7" >0.42</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row14" class="row_heading level0 row14" >2013</th>
      <td id="T_a6940_row14_col0" class="data row14 col0" >3.9</td>
      <td id="T_a6940_row14_col1" class="data row14 col1" >0.28</td>
      <td id="T_a6940_row14_col2" class="data row14 col2" >202,030</td>
      <td id="T_a6940_row14_col3" class="data row14 col3" >75.5</td>
      <td id="T_a6940_row14_col4" class="data row14 col4" >32.1</td>
      <td id="T_a6940_row14_col5" class="data row14 col5" >752</td>
      <td id="T_a6940_row14_col6" class="data row14 col6" >0.09</td>
      <td id="T_a6940_row14_col7" class="data row14 col7" >0.45</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row15" class="row_heading level0 row15" >2014</th>
      <td id="T_a6940_row15_col0" class="data row15 col0" >4.3</td>
      <td id="T_a6940_row15_col1" class="data row15 col1" >0.50</td>
      <td id="T_a6940_row15_col2" class="data row15 col2" >208,098</td>
      <td id="T_a6940_row15_col3" class="data row15 col3" >74.3</td>
      <td id="T_a6940_row15_col4" class="data row15 col4" >33.7</td>
      <td id="T_a6940_row15_col5" class="data row15 col5" >746</td>
      <td id="T_a6940_row15_col6" class="data row15 col6" >0.10</td>
      <td id="T_a6940_row15_col7" class="data row15 col7" >0.49</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row16" class="row_heading level0 row16" >2015</th>
      <td id="T_a6940_row16_col0" class="data row16 col0" >4.0</td>
      <td id="T_a6940_row16_col1" class="data row16 col1" >0.43</td>
      <td id="T_a6940_row16_col2" class="data row16 col2" >226,881</td>
      <td id="T_a6940_row16_col3" class="data row16 col3" >73.5</td>
      <td id="T_a6940_row16_col4" class="data row16 col4" >33.5</td>
      <td id="T_a6940_row16_col5" class="data row16 col5" >750</td>
      <td id="T_a6940_row16_col6" class="data row16 col6" >0.09</td>
      <td id="T_a6940_row16_col7" class="data row16 col7" >0.48</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row17" class="row_heading level0 row17" >2016</th>
      <td id="T_a6940_row17_col0" class="data row17 col0" >3.8</td>
      <td id="T_a6940_row17_col1" class="data row17 col1" >0.45</td>
      <td id="T_a6940_row17_col2" class="data row17 col2" >236,026</td>
      <td id="T_a6940_row17_col3" class="data row17 col3" >73.3</td>
      <td id="T_a6940_row17_col4" class="data row17 col4" >33.8</td>
      <td id="T_a6940_row17_col5" class="data row17 col5" >748</td>
      <td id="T_a6940_row17_col6" class="data row17 col6" >0.09</td>
      <td id="T_a6940_row17_col7" class="data row17 col7" >0.49</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row18" class="row_heading level0 row18" >2017</th>
      <td id="T_a6940_row18_col0" class="data row18 col0" >4.2</td>
      <td id="T_a6940_row18_col1" class="data row18 col1" >0.56</td>
      <td id="T_a6940_row18_col2" class="data row18 col2" >229,910</td>
      <td id="T_a6940_row18_col3" class="data row18 col3" >75.1</td>
      <td id="T_a6940_row18_col4" class="data row18 col4" >34.9</td>
      <td id="T_a6940_row18_col5" class="data row18 col5" >745</td>
      <td id="T_a6940_row18_col6" class="data row18 col6" >0.10</td>
      <td id="T_a6940_row18_col7" class="data row18 col7" >0.53</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row19" class="row_heading level0 row19" >2018</th>
      <td id="T_a6940_row19_col0" class="data row19 col0" >4.7</td>
      <td id="T_a6940_row19_col1" class="data row19 col1" >0.67</td>
      <td id="T_a6940_row19_col2" class="data row19 col2" >229,418</td>
      <td id="T_a6940_row19_col3" class="data row19 col3" >75.8</td>
      <td id="T_a6940_row19_col4" class="data row19 col4" >35.8</td>
      <td id="T_a6940_row19_col5" class="data row19 col5" >747</td>
      <td id="T_a6940_row19_col6" class="data row19 col6" >0.10</td>
      <td id="T_a6940_row19_col7" class="data row19 col7" >0.54</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row20" class="row_heading level0 row20" >2019</th>
      <td id="T_a6940_row20_col0" class="data row20 col0" >4.2</td>
      <td id="T_a6940_row20_col1" class="data row20 col1" >0.57</td>
      <td id="T_a6940_row20_col2" class="data row20 col2" >255,544</td>
      <td id="T_a6940_row20_col3" class="data row20 col3" >76.2</td>
      <td id="T_a6940_row20_col4" class="data row20 col4" >35.3</td>
      <td id="T_a6940_row20_col5" class="data row20 col5" >750</td>
      <td id="T_a6940_row20_col6" class="data row20 col6" >0.08</td>
      <td id="T_a6940_row20_col7" class="data row20 col7" >0.54</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row21" class="row_heading level0 row21" >2020</th>
      <td id="T_a6940_row21_col0" class="data row21 col0" >3.2</td>
      <td id="T_a6940_row21_col1" class="data row21 col1" >0.29</td>
      <td id="T_a6940_row21_col2" class="data row21 col2" >290,616</td>
      <td id="T_a6940_row21_col3" class="data row21 col3" >71.1</td>
      <td id="T_a6940_row21_col4" class="data row21 col4" >33.3</td>
      <td id="T_a6940_row21_col5" class="data row21 col5" >758</td>
      <td id="T_a6940_row21_col6" class="data row21 col6" >0.05</td>
      <td id="T_a6940_row21_col7" class="data row21 col7" >0.50</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row22" class="row_heading level0 row22" >2021</th>
      <td id="T_a6940_row22_col0" class="data row22 col0" >3.0</td>
      <td id="T_a6940_row22_col1" class="data row22 col1" >0.35</td>
      <td id="T_a6940_row22_col2" class="data row22 col2" >291,863</td>
      <td id="T_a6940_row22_col3" class="data row22 col3" >69.3</td>
      <td id="T_a6940_row22_col4" class="data row22 col4" >34.1</td>
      <td id="T_a6940_row22_col5" class="data row22 col5" >751</td>
      <td id="T_a6940_row22_col6" class="data row22 col6" >0.08</td>
      <td id="T_a6940_row22_col7" class="data row22 col7" >0.54</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row23" class="row_heading level0 row23" >2022</th>
      <td id="T_a6940_row23_col0" class="data row23 col0" >5.1</td>
      <td id="T_a6940_row23_col1" class="data row23 col1" >0.67</td>
      <td id="T_a6940_row23_col2" class="data row23 col2" >302,796</td>
      <td id="T_a6940_row23_col3" class="data row23 col3" >73.9</td>
      <td id="T_a6940_row23_col4" class="data row23 col4" >37.2</td>
      <td id="T_a6940_row23_col5" class="data row23 col5" >744</td>
      <td id="T_a6940_row23_col6" class="data row23 col6" >0.10</td>
      <td id="T_a6940_row23_col7" class="data row23 col7" >0.54</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row24" class="row_heading level0 row24" >2023</th>
      <td id="T_a6940_row24_col0" class="data row24 col0" >6.3</td>
      <td id="T_a6940_row24_col1" class="data row24 col1" >0.83</td>
      <td id="T_a6940_row24_col2" class="data row24 col2" >299,611</td>
      <td id="T_a6940_row24_col3" class="data row24 col3" >76.9</td>
      <td id="T_a6940_row24_col4" class="data row24 col4" >38.4</td>
      <td id="T_a6940_row24_col5" class="data row24 col5" >749</td>
      <td id="T_a6940_row24_col6" class="data row24 col6" >0.08</td>
      <td id="T_a6940_row24_col7" class="data row24 col7" >0.62</td>
    </tr>
    <tr>
      <th id="T_a6940_level0_row25" class="row_heading level0 row25" >Overall Average</th>
      <td id="T_a6940_row25_col0" class="data row25 col0" >5.1</td>
      <td id="T_a6940_row25_col1" class="data row25 col1" >0.40</td>
      <td id="T_a6940_row25_col2" class="data row25 col2" >203,572</td>
      <td id="T_a6940_row25_col3" class="data row25 col3" >71.7</td>
      <td id="T_a6940_row25_col4" class="data row25 col4" >33.8</td>
      <td id="T_a6940_row25_col5" class="data row25 col5" >741</td>
      <td id="T_a6940_row25_col6" class="data row25 col6" >0.14</td>
      <td id="T_a6940_row25_col7" class="data row25 col7" >0.44</td>
    </tr>
  </tbody>
</table>




## Question 4

#### Renaming Variables


```python
df1 = df1.rename(columns={'SELLER NAME': 'Seller'})
```

#### Creating the Table


```python
#Creating the initial dataframe only with the counts for each seller, and calling the column Frequency
seller_loan_counts = df1.groupby("Seller").size().reset_index(name = "Frequency")

#Sorting the dataframe in descending order
seller_loan_counts_sorted = seller_loan_counts.sort_values(by='Frequency', ascending=False)

#Creating the Percent column
seller_loan_counts_sorted['Percent'] = (
    (seller_loan_counts_sorted['Frequency'] / seller_loan_counts_sorted['Frequency'].sum()) * 100
)

#Creating the Number column
seller_loan_counts_sorted['Number'] = range(1, len(seller_loan_counts_sorted) + 1)

#Creating the cumulative percent column
seller_loan_counts_sorted['Cumulative'] = seller_loan_counts_sorted['Percent'].cumsum()

#Ordering the columns for display
seller_loan_counts_sorted = seller_loan_counts_sorted[['Number', 'Seller', 'Frequency', 'Percent', "Cumulative"]]

#Changing display options so that all rows are displayed
pd.set_option("display.max_rows", None)

#Making the Number the index of the dataframe
seller_loan_counts_sorted.set_index('Number', inplace=True)
```

#### Polishing the table


```python
formatting_rules = {
    "Percent": '{:.2f}',
    "Cumulative": '{:.2f}',
}

seller_loan_counts_sorted_formatted = seller_loan_counts_sorted.style.format(formatting_rules)
```

### Final Question 4 Table


```python
seller_loan_counts_sorted_formatted
```




<style type="text/css">
</style>
<table id="T_7fa37">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_7fa37_level0_col0" class="col_heading level0 col0" >Seller</th>
      <th id="T_7fa37_level0_col1" class="col_heading level0 col1" >Frequency</th>
      <th id="T_7fa37_level0_col2" class="col_heading level0 col2" >Percent</th>
      <th id="T_7fa37_level0_col3" class="col_heading level0 col3" >Cumulative</th>
    </tr>
    <tr>
      <th class="index_name level0" >Number</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_7fa37_level0_row0" class="row_heading level0 row0" >1</th>
      <td id="T_7fa37_row0_col0" class="data row0 col0" >Other sellers</td>
      <td id="T_7fa37_row0_col1" class="data row0 col1" >18989</td>
      <td id="T_7fa37_row0_col2" class="data row0 col2" >26.10</td>
      <td id="T_7fa37_row0_col3" class="data row0 col3" >26.10</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row1" class="row_heading level0 row1" >2</th>
      <td id="T_7fa37_row1_col0" class="data row1 col0" >WELLS FARGO BANK, N.A.</td>
      <td id="T_7fa37_row1_col1" class="data row1 col1" >10152</td>
      <td id="T_7fa37_row1_col2" class="data row1 col2" >13.95</td>
      <td id="T_7fa37_row1_col3" class="data row1 col3" >40.06</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row2" class="row_heading level0 row2" >3</th>
      <td id="T_7fa37_row2_col0" class="data row2 col0" >WELLS FARGO HOME MORTGAGE, INC.</td>
      <td id="T_7fa37_row2_col1" class="data row2 col1" >3667</td>
      <td id="T_7fa37_row2_col2" class="data row2 col2" >5.04</td>
      <td id="T_7fa37_row2_col3" class="data row2 col3" >45.10</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row3" class="row_heading level0 row3" >4</th>
      <td id="T_7fa37_row3_col0" class="data row3 col0" >BANK OF AMERICA, N.A.</td>
      <td id="T_7fa37_row3_col1" class="data row3 col1" >3557</td>
      <td id="T_7fa37_row3_col2" class="data row3 col2" >4.89</td>
      <td id="T_7fa37_row3_col3" class="data row3 col3" >49.99</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row4" class="row_heading level0 row4" >5</th>
      <td id="T_7fa37_row4_col0" class="data row4 col0" >U.S. BANK N.A.</td>
      <td id="T_7fa37_row4_col1" class="data row4 col1" >3168</td>
      <td id="T_7fa37_row4_col2" class="data row4 col2" >4.35</td>
      <td id="T_7fa37_row4_col3" class="data row4 col3" >54.34</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row5" class="row_heading level0 row5" >6</th>
      <td id="T_7fa37_row5_col0" class="data row5 col0" >ABN AMRO MORTGAGE GROUP, INC.</td>
      <td id="T_7fa37_row5_col1" class="data row5 col1" >2435</td>
      <td id="T_7fa37_row5_col2" class="data row5 col2" >3.35</td>
      <td id="T_7fa37_row5_col3" class="data row5 col3" >57.69</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row6" class="row_heading level0 row6" >7</th>
      <td id="T_7fa37_row6_col0" class="data row6 col0" >JPMORGAN CHASE BANK, N.A.</td>
      <td id="T_7fa37_row6_col1" class="data row6 col1" >2092</td>
      <td id="T_7fa37_row6_col2" class="data row6 col2" >2.88</td>
      <td id="T_7fa37_row6_col3" class="data row6 col3" >60.56</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row7" class="row_heading level0 row7" >8</th>
      <td id="T_7fa37_row7_col0" class="data row7 col0" >CHASE HOME FINANCE LLC</td>
      <td id="T_7fa37_row7_col1" class="data row7 col1" >1658</td>
      <td id="T_7fa37_row7_col2" class="data row7 col2" >2.28</td>
      <td id="T_7fa37_row7_col3" class="data row7 col3" >62.84</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row8" class="row_heading level0 row8" >9</th>
      <td id="T_7fa37_row8_col0" class="data row8 col0" >BRANCH BANKING & TRUST COMPANY</td>
      <td id="T_7fa37_row8_col1" class="data row8 col1" >1511</td>
      <td id="T_7fa37_row8_col2" class="data row8 col2" >2.08</td>
      <td id="T_7fa37_row8_col3" class="data row8 col3" >64.92</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row9" class="row_heading level0 row9" >10</th>
      <td id="T_7fa37_row9_col0" class="data row9 col0" >COUNTRYWIDE HOME LOANS, INC.</td>
      <td id="T_7fa37_row9_col1" class="data row9 col1" >1383</td>
      <td id="T_7fa37_row9_col2" class="data row9 col2" >1.90</td>
      <td id="T_7fa37_row9_col3" class="data row9 col3" >66.82</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row10" class="row_heading level0 row10" >11</th>
      <td id="T_7fa37_row10_col0" class="data row10 col0" >QUICKEN LOANS INC.</td>
      <td id="T_7fa37_row10_col1" class="data row10 col1" >1306</td>
      <td id="T_7fa37_row10_col2" class="data row10 col2" >1.80</td>
      <td id="T_7fa37_row10_col3" class="data row10 col3" >68.62</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row11" class="row_heading level0 row11" >12</th>
      <td id="T_7fa37_row11_col0" class="data row11 col0" >CITIMORTGAGE, INC.</td>
      <td id="T_7fa37_row11_col1" class="data row11 col1" >1095</td>
      <td id="T_7fa37_row11_col2" class="data row11 col2" >1.51</td>
      <td id="T_7fa37_row11_col3" class="data row11 col3" >70.12</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row12" class="row_heading level0 row12" >13</th>
      <td id="T_7fa37_row12_col0" class="data row12 col0" >WASHINGTON MUTUAL BANK</td>
      <td id="T_7fa37_row12_col1" class="data row12 col1" >1075</td>
      <td id="T_7fa37_row12_col2" class="data row12 col2" >1.48</td>
      <td id="T_7fa37_row12_col3" class="data row12 col3" >71.60</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row13" class="row_heading level0 row13" >14</th>
      <td id="T_7fa37_row13_col0" class="data row13 col0" >FIFTH THIRD BANK</td>
      <td id="T_7fa37_row13_col1" class="data row13 col1" >1073</td>
      <td id="T_7fa37_row13_col2" class="data row13 col2" >1.47</td>
      <td id="T_7fa37_row13_col3" class="data row13 col3" >73.07</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row14" class="row_heading level0 row14" >15</th>
      <td id="T_7fa37_row14_col0" class="data row14 col0" >CHASE MANHATTAN MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row14_col1" class="data row14 col1" >932</td>
      <td id="T_7fa37_row14_col2" class="data row14 col2" >1.28</td>
      <td id="T_7fa37_row14_col3" class="data row14 col3" >74.35</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row15" class="row_heading level0 row15" >16</th>
      <td id="T_7fa37_row15_col0" class="data row15 col0" >AMERIHOME MORTGAGE COMPANY, LLC</td>
      <td id="T_7fa37_row15_col1" class="data row15 col1" >926</td>
      <td id="T_7fa37_row15_col2" class="data row15 col2" >1.27</td>
      <td id="T_7fa37_row15_col3" class="data row15 col3" >75.63</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row16" class="row_heading level0 row16" >17</th>
      <td id="T_7fa37_row16_col0" class="data row16 col0" >JPMORGAN CHASE BANK, NATIONAL ASSOCIATION</td>
      <td id="T_7fa37_row16_col1" class="data row16 col1" >856</td>
      <td id="T_7fa37_row16_col2" class="data row16 col2" >1.18</td>
      <td id="T_7fa37_row16_col3" class="data row16 col3" >76.80</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row17" class="row_heading level0 row17" >18</th>
      <td id="T_7fa37_row17_col0" class="data row17 col0" >NORWEST MORTGAGE, INC.</td>
      <td id="T_7fa37_row17_col1" class="data row17 col1" >837</td>
      <td id="T_7fa37_row17_col2" class="data row17 col2" >1.15</td>
      <td id="T_7fa37_row17_col3" class="data row17 col3" >77.95</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row18" class="row_heading level0 row18" >19</th>
      <td id="T_7fa37_row18_col0" class="data row18 col0" >PRINCIPAL RESIDENTIAL MORTGAGE, INC.</td>
      <td id="T_7fa37_row18_col1" class="data row18 col1" >829</td>
      <td id="T_7fa37_row18_col2" class="data row18 col2" >1.14</td>
      <td id="T_7fa37_row18_col3" class="data row18 col3" >79.09</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row19" class="row_heading level0 row19" >20</th>
      <td id="T_7fa37_row19_col0" class="data row19 col0" >CALIBER HOME LOANS, INC.</td>
      <td id="T_7fa37_row19_col1" class="data row19 col1" >801</td>
      <td id="T_7fa37_row19_col2" class="data row19 col2" >1.10</td>
      <td id="T_7fa37_row19_col3" class="data row19 col3" >80.20</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row20" class="row_heading level0 row20" >21</th>
      <td id="T_7fa37_row20_col0" class="data row20 col0" >PROVIDENT FUNDING ASSOCIATES, L.P.</td>
      <td id="T_7fa37_row20_col1" class="data row20 col1" >783</td>
      <td id="T_7fa37_row20_col2" class="data row20 col2" >1.08</td>
      <td id="T_7fa37_row20_col3" class="data row20 col3" >81.27</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row21" class="row_heading level0 row21" >22</th>
      <td id="T_7fa37_row21_col0" class="data row21 col0" >NATIONAL CITY MORTGAGE CO.</td>
      <td id="T_7fa37_row21_col1" class="data row21 col1" >751</td>
      <td id="T_7fa37_row21_col2" class="data row21 col2" >1.03</td>
      <td id="T_7fa37_row21_col3" class="data row21 col3" >82.30</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row22" class="row_heading level0 row22" >23</th>
      <td id="T_7fa37_row22_col0" class="data row22 col0" >UNITED WHOLESALE MORTGAGE, LLC</td>
      <td id="T_7fa37_row22_col1" class="data row22 col1" >669</td>
      <td id="T_7fa37_row22_col2" class="data row22 col2" >0.92</td>
      <td id="T_7fa37_row22_col3" class="data row22 col3" >83.22</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row23" class="row_heading level0 row23" >24</th>
      <td id="T_7fa37_row23_col0" class="data row23 col0" >LOANDEPOT.COM, LLC</td>
      <td id="T_7fa37_row23_col1" class="data row23 col1" >661</td>
      <td id="T_7fa37_row23_col2" class="data row23 col2" >0.91</td>
      <td id="T_7fa37_row23_col3" class="data row23 col3" >84.13</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row24" class="row_heading level0 row24" >25</th>
      <td id="T_7fa37_row24_col0" class="data row24 col0" >FLAGSTAR BANK, FSB</td>
      <td id="T_7fa37_row24_col1" class="data row24 col1" >659</td>
      <td id="T_7fa37_row24_col2" class="data row24 col2" >0.91</td>
      <td id="T_7fa37_row24_col3" class="data row24 col3" >85.04</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row25" class="row_heading level0 row25" >26</th>
      <td id="T_7fa37_row25_col0" class="data row25 col0" >TAYLOR, BEAN & WHITAKER MORTGAGE CORP.</td>
      <td id="T_7fa37_row25_col1" class="data row25 col1" >608</td>
      <td id="T_7fa37_row25_col2" class="data row25 col2" >0.84</td>
      <td id="T_7fa37_row25_col3" class="data row25 col3" >85.87</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row26" class="row_heading level0 row26" >27</th>
      <td id="T_7fa37_row26_col0" class="data row26 col0" >PENNYMAC CORP.</td>
      <td id="T_7fa37_row26_col1" class="data row26 col1" >569</td>
      <td id="T_7fa37_row26_col2" class="data row26 col2" >0.78</td>
      <td id="T_7fa37_row26_col3" class="data row26 col3" >86.66</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row27" class="row_heading level0 row27" >28</th>
      <td id="T_7fa37_row27_col0" class="data row27 col0" >GMAC MORTGAGE, LLC</td>
      <td id="T_7fa37_row27_col1" class="data row27 col1" >563</td>
      <td id="T_7fa37_row27_col2" class="data row27 col2" >0.77</td>
      <td id="T_7fa37_row27_col3" class="data row27 col3" >87.43</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row28" class="row_heading level0 row28" >29</th>
      <td id="T_7fa37_row28_col0" class="data row28 col0" >SUNTRUST MORTGAGE, INC.</td>
      <td id="T_7fa37_row28_col1" class="data row28 col1" >552</td>
      <td id="T_7fa37_row28_col2" class="data row28 col2" >0.76</td>
      <td id="T_7fa37_row28_col3" class="data row28 col3" >88.19</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row29" class="row_heading level0 row29" >30</th>
      <td id="T_7fa37_row29_col0" class="data row29 col0" >GMAC MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row29_col1" class="data row29 col1" >506</td>
      <td id="T_7fa37_row29_col2" class="data row29 col2" >0.70</td>
      <td id="T_7fa37_row29_col3" class="data row29 col3" >88.88</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row30" class="row_heading level0 row30" >31</th>
      <td id="T_7fa37_row30_col0" class="data row30 col0" >FAIRWAY INDEPENDENT MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row30_col1" class="data row30 col1" >484</td>
      <td id="T_7fa37_row30_col2" class="data row30 col2" >0.67</td>
      <td id="T_7fa37_row30_col3" class="data row30 col3" >89.55</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row31" class="row_heading level0 row31" >32</th>
      <td id="T_7fa37_row31_col0" class="data row31 col0" >GUARANTEED RATE, INC.</td>
      <td id="T_7fa37_row31_col1" class="data row31 col1" >427</td>
      <td id="T_7fa37_row31_col2" class="data row31 col2" >0.59</td>
      <td id="T_7fa37_row31_col3" class="data row31 col3" >90.14</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row32" class="row_heading level0 row32" >33</th>
      <td id="T_7fa37_row32_col0" class="data row32 col0" >UNITED SHORE FINANCIAL SERVICES, LLC</td>
      <td id="T_7fa37_row32_col1" class="data row32 col1" >424</td>
      <td id="T_7fa37_row32_col2" class="data row32 col2" >0.58</td>
      <td id="T_7fa37_row32_col3" class="data row32 col3" >90.72</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row33" class="row_heading level0 row33" >34</th>
      <td id="T_7fa37_row33_col0" class="data row33 col0" >NATIONSTAR MORTGAGE LLC DBA MR. COOPER</td>
      <td id="T_7fa37_row33_col1" class="data row33 col1" >415</td>
      <td id="T_7fa37_row33_col2" class="data row33 col2" >0.57</td>
      <td id="T_7fa37_row33_col3" class="data row33 col3" >91.29</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row34" class="row_heading level0 row34" >35</th>
      <td id="T_7fa37_row34_col0" class="data row34 col0" >ROCKET MORTGAGE, LLC</td>
      <td id="T_7fa37_row34_col1" class="data row34 col1" >403</td>
      <td id="T_7fa37_row34_col2" class="data row34 col2" >0.55</td>
      <td id="T_7fa37_row34_col3" class="data row34 col3" >91.84</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row35" class="row_heading level0 row35" >36</th>
      <td id="T_7fa37_row35_col0" class="data row35 col0" >QUICKEN LOANS, LLC</td>
      <td id="T_7fa37_row35_col1" class="data row35 col1" >328</td>
      <td id="T_7fa37_row35_col2" class="data row35 col2" >0.45</td>
      <td id="T_7fa37_row35_col3" class="data row35 col3" >92.29</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row36" class="row_heading level0 row36" >37</th>
      <td id="T_7fa37_row36_col0" class="data row36 col0" >CITIZENS BANK, NA</td>
      <td id="T_7fa37_row36_col1" class="data row36 col1" >291</td>
      <td id="T_7fa37_row36_col2" class="data row36 col2" >0.40</td>
      <td id="T_7fa37_row36_col3" class="data row36 col3" >92.69</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row37" class="row_heading level0 row37" >38</th>
      <td id="T_7fa37_row37_col0" class="data row37 col0" >NEWREZ LLC</td>
      <td id="T_7fa37_row37_col1" class="data row37 col1" >275</td>
      <td id="T_7fa37_row37_col2" class="data row37 col2" >0.38</td>
      <td id="T_7fa37_row37_col3" class="data row37 col3" >93.07</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row38" class="row_heading level0 row38" >39</th>
      <td id="T_7fa37_row38_col0" class="data row38 col0" >FRANKLIN AMERICAN MORTGAGE COMPANY</td>
      <td id="T_7fa37_row38_col1" class="data row38 col1" >274</td>
      <td id="T_7fa37_row38_col2" class="data row38 col2" >0.38</td>
      <td id="T_7fa37_row38_col3" class="data row38 col3" >93.45</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row39" class="row_heading level0 row39" >40</th>
      <td id="T_7fa37_row39_col0" class="data row39 col0" >NATIONSTAR MORTGAGE LLC</td>
      <td id="T_7fa37_row39_col1" class="data row39 col1" >241</td>
      <td id="T_7fa37_row39_col2" class="data row39 col2" >0.33</td>
      <td id="T_7fa37_row39_col3" class="data row39 col3" >93.78</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row40" class="row_heading level0 row40" >41</th>
      <td id="T_7fa37_row40_col0" class="data row40 col0" >HOME POINT FINANCIAL CORPORATION</td>
      <td id="T_7fa37_row40_col1" class="data row40 col1" >228</td>
      <td id="T_7fa37_row40_col2" class="data row40 col2" >0.31</td>
      <td id="T_7fa37_row40_col3" class="data row40 col3" >94.09</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row41" class="row_heading level0 row41" >42</th>
      <td id="T_7fa37_row41_col0" class="data row41 col0" >TRUIST BANK</td>
      <td id="T_7fa37_row41_col1" class="data row41 col1" >219</td>
      <td id="T_7fa37_row41_col2" class="data row41 col2" >0.30</td>
      <td id="T_7fa37_row41_col3" class="data row41 col3" >94.39</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row42" class="row_heading level0 row42" >43</th>
      <td id="T_7fa37_row42_col0" class="data row42 col0" >METLIFE HOME LOANS, A DIVISION OF METLIFE BANK, N.A.</td>
      <td id="T_7fa37_row42_col1" class="data row42 col1" >216</td>
      <td id="T_7fa37_row42_col2" class="data row42 col2" >0.30</td>
      <td id="T_7fa37_row42_col3" class="data row42 col3" >94.69</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row43" class="row_heading level0 row43" >44</th>
      <td id="T_7fa37_row43_col0" class="data row43 col0" >STEARNS LENDING, LLC</td>
      <td id="T_7fa37_row43_col1" class="data row43 col1" >215</td>
      <td id="T_7fa37_row43_col2" class="data row43 col2" >0.30</td>
      <td id="T_7fa37_row43_col3" class="data row43 col3" >94.99</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row44" class="row_heading level0 row44" >45</th>
      <td id="T_7fa37_row44_col0" class="data row44 col0" >WASHINGTON MUTUAL BANK, F.A.</td>
      <td id="T_7fa37_row44_col1" class="data row44 col1" >204</td>
      <td id="T_7fa37_row44_col2" class="data row44 col2" >0.28</td>
      <td id="T_7fa37_row44_col3" class="data row44 col3" >95.27</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row45" class="row_heading level0 row45" >46</th>
      <td id="T_7fa37_row45_col0" class="data row45 col0" >UNITED SHORE FINANCIAL SERVICES, LLC, DBA UNITED WHOLESALE M</td>
      <td id="T_7fa37_row45_col1" class="data row45 col1" >203</td>
      <td id="T_7fa37_row45_col2" class="data row45 col2" >0.28</td>
      <td id="T_7fa37_row45_col3" class="data row45 col3" >95.55</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row46" class="row_heading level0 row46" >47</th>
      <td id="T_7fa37_row46_col0" class="data row46 col0" >PHH MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row46_col1" class="data row46 col1" >177</td>
      <td id="T_7fa37_row46_col2" class="data row46 col2" >0.24</td>
      <td id="T_7fa37_row46_col3" class="data row46 col3" >95.79</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row47" class="row_heading level0 row47" >48</th>
      <td id="T_7fa37_row47_col0" class="data row47 col0" >PENNYMAC LOAN SERVICES, LLC</td>
      <td id="T_7fa37_row47_col1" class="data row47 col1" >171</td>
      <td id="T_7fa37_row47_col2" class="data row47 col2" >0.24</td>
      <td id="T_7fa37_row47_col3" class="data row47 col3" >96.02</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row48" class="row_heading level0 row48" >49</th>
      <td id="T_7fa37_row48_col0" class="data row48 col0" >CENDANT MORTGAGE CORPORATION, DBA PHH MORTGAGE SERVICES, INC</td>
      <td id="T_7fa37_row48_col1" class="data row48 col1" >166</td>
      <td id="T_7fa37_row48_col2" class="data row48 col2" >0.23</td>
      <td id="T_7fa37_row48_col3" class="data row48 col3" >96.25</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row49" class="row_heading level0 row49" >50</th>
      <td id="T_7fa37_row49_col0" class="data row49 col0" >COUNTRYWIDE BANK, FSB</td>
      <td id="T_7fa37_row49_col1" class="data row49 col1" >155</td>
      <td id="T_7fa37_row49_col2" class="data row49 col2" >0.21</td>
      <td id="T_7fa37_row49_col3" class="data row49 col3" >96.47</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row50" class="row_heading level0 row50" >51</th>
      <td id="T_7fa37_row50_col0" class="data row50 col0" >HSBC BANK USA</td>
      <td id="T_7fa37_row50_col1" class="data row50 col1" >136</td>
      <td id="T_7fa37_row50_col2" class="data row50 col2" >0.19</td>
      <td id="T_7fa37_row50_col3" class="data row50 col3" >96.65</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row51" class="row_heading level0 row51" >52</th>
      <td id="T_7fa37_row51_col0" class="data row51 col0" >UNITED SHORE FINANCIAL SERVICES, LLC., DBA SHORE MORTGAGE</td>
      <td id="T_7fa37_row51_col1" class="data row51 col1" >118</td>
      <td id="T_7fa37_row51_col2" class="data row51 col2" >0.16</td>
      <td id="T_7fa37_row51_col3" class="data row51 col3" >96.82</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row52" class="row_heading level0 row52" >53</th>
      <td id="T_7fa37_row52_col0" class="data row52 col0" >AMTRUST BANK</td>
      <td id="T_7fa37_row52_col1" class="data row52 col1" >117</td>
      <td id="T_7fa37_row52_col2" class="data row52 col2" >0.16</td>
      <td id="T_7fa37_row52_col3" class="data row52 col3" >96.98</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row53" class="row_heading level0 row53" >54</th>
      <td id="T_7fa37_row53_col0" class="data row53 col0" >UNION SAVINGS BANK</td>
      <td id="T_7fa37_row53_col1" class="data row53 col1" >107</td>
      <td id="T_7fa37_row53_col2" class="data row53 col2" >0.15</td>
      <td id="T_7fa37_row53_col3" class="data row53 col3" >97.12</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row54" class="row_heading level0 row54" >55</th>
      <td id="T_7fa37_row54_col0" class="data row54 col0" >LASALLE BANK, FSB</td>
      <td id="T_7fa37_row54_col1" class="data row54 col1" >95</td>
      <td id="T_7fa37_row54_col2" class="data row54 col2" >0.13</td>
      <td id="T_7fa37_row54_col3" class="data row54 col3" >97.25</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row55" class="row_heading level0 row55" >56</th>
      <td id="T_7fa37_row55_col0" class="data row55 col0" >CMG MORTGAGE, INC.</td>
      <td id="T_7fa37_row55_col1" class="data row55 col1" >93</td>
      <td id="T_7fa37_row55_col2" class="data row55 col2" >0.13</td>
      <td id="T_7fa37_row55_col3" class="data row55 col3" >97.38</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row56" class="row_heading level0 row56" >57</th>
      <td id="T_7fa37_row56_col0" class="data row56 col0" >PRIMELENDING A PLAINS CAPITAL CO</td>
      <td id="T_7fa37_row56_col1" class="data row56 col1" >92</td>
      <td id="T_7fa37_row56_col2" class="data row56 col2" >0.13</td>
      <td id="T_7fa37_row56_col3" class="data row56 col3" >97.51</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row57" class="row_heading level0 row57" >58</th>
      <td id="T_7fa37_row57_col0" class="data row57 col0" >NATIONSBANK, N.A.,DBA BANK OF AMERICA MORTGAGE</td>
      <td id="T_7fa37_row57_col1" class="data row57 col1" >90</td>
      <td id="T_7fa37_row57_col2" class="data row57 col2" >0.12</td>
      <td id="T_7fa37_row57_col3" class="data row57 col3" >97.63</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row58" class="row_heading level0 row58" >59</th>
      <td id="T_7fa37_row58_col0" class="data row58 col0" >PNC BANK, NA</td>
      <td id="T_7fa37_row58_col1" class="data row58 col1" >83</td>
      <td id="T_7fa37_row58_col2" class="data row58 col2" >0.11</td>
      <td id="T_7fa37_row58_col3" class="data row58 col3" >97.75</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row59" class="row_heading level0 row59" >60</th>
      <td id="T_7fa37_row59_col0" class="data row59 col0" >PRIMELENDING, A PLAINSCAPITAL COMPANY</td>
      <td id="T_7fa37_row59_col1" class="data row59 col1" >82</td>
      <td id="T_7fa37_row59_col2" class="data row59 col2" >0.11</td>
      <td id="T_7fa37_row59_col3" class="data row59 col3" >97.86</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row60" class="row_heading level0 row60" >61</th>
      <td id="T_7fa37_row60_col0" class="data row60 col0" >SUNTRUST BANK</td>
      <td id="T_7fa37_row60_col1" class="data row60 col1" >82</td>
      <td id="T_7fa37_row60_col2" class="data row60 col2" >0.11</td>
      <td id="T_7fa37_row60_col3" class="data row60 col3" >97.97</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row61" class="row_heading level0 row61" >62</th>
      <td id="T_7fa37_row61_col0" class="data row61 col0" >FIRSTAR BANK, N.A.</td>
      <td id="T_7fa37_row61_col1" class="data row61 col1" >81</td>
      <td id="T_7fa37_row61_col2" class="data row61 col2" >0.11</td>
      <td id="T_7fa37_row61_col3" class="data row61 col3" >98.08</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row62" class="row_heading level0 row62" >63</th>
      <td id="T_7fa37_row62_col0" class="data row62 col0" >U.S. BANK, N.A.</td>
      <td id="T_7fa37_row62_col1" class="data row62 col1" >81</td>
      <td id="T_7fa37_row62_col2" class="data row62 col2" >0.11</td>
      <td id="T_7fa37_row62_col3" class="data row62 col3" >98.19</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row63" class="row_heading level0 row63" >64</th>
      <td id="T_7fa37_row63_col0" class="data row63 col0" >NATIONAL CITY MORTGAGE COMPANY</td>
      <td id="T_7fa37_row63_col1" class="data row63 col1" >79</td>
      <td id="T_7fa37_row63_col2" class="data row63 col2" >0.11</td>
      <td id="T_7fa37_row63_col3" class="data row63 col3" >98.30</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row64" class="row_heading level0 row64" >65</th>
      <td id="T_7fa37_row64_col0" class="data row64 col0" >GUILD MORTGAGE COMPANY LLC</td>
      <td id="T_7fa37_row64_col1" class="data row64 col1" >68</td>
      <td id="T_7fa37_row64_col2" class="data row64 col2" >0.09</td>
      <td id="T_7fa37_row64_col3" class="data row64 col3" >98.40</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row65" class="row_heading level0 row65" >66</th>
      <td id="T_7fa37_row65_col0" class="data row65 col0" >GE CAPITAL MORTGAGE SERVICES, INC.</td>
      <td id="T_7fa37_row65_col1" class="data row65 col1" >66</td>
      <td id="T_7fa37_row65_col2" class="data row65 col2" >0.09</td>
      <td id="T_7fa37_row65_col3" class="data row65 col3" >98.49</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row66" class="row_heading level0 row66" >67</th>
      <td id="T_7fa37_row66_col0" class="data row66 col0" >FINANCE OF AMERICA MORTGAGE LLC</td>
      <td id="T_7fa37_row66_col1" class="data row66 col1" >65</td>
      <td id="T_7fa37_row66_col2" class="data row66 col2" >0.09</td>
      <td id="T_7fa37_row66_col3" class="data row66 col3" >98.58</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row67" class="row_heading level0 row67" >68</th>
      <td id="T_7fa37_row67_col0" class="data row67 col0" >FIRST UNION MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row67_col1" class="data row67 col1" >62</td>
      <td id="T_7fa37_row67_col2" class="data row67 col2" >0.09</td>
      <td id="T_7fa37_row67_col3" class="data row67 col3" >98.66</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row68" class="row_heading level0 row68" >69</th>
      <td id="T_7fa37_row68_col0" class="data row68 col0" >CROSSCOUNTRY MORTGAGE, LLC</td>
      <td id="T_7fa37_row68_col1" class="data row68 col1" >51</td>
      <td id="T_7fa37_row68_col2" class="data row68 col2" >0.07</td>
      <td id="T_7fa37_row68_col3" class="data row68 col3" >98.73</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row69" class="row_heading level0 row69" >70</th>
      <td id="T_7fa37_row69_col0" class="data row69 col0" >FIRST HORIZON HOME LOANS, A DIVISION OF FIRST TENNESSEE BANK</td>
      <td id="T_7fa37_row69_col1" class="data row69 col1" >51</td>
      <td id="T_7fa37_row69_col2" class="data row69 col2" >0.07</td>
      <td id="T_7fa37_row69_col3" class="data row69 col3" >98.80</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row70" class="row_heading level0 row70" >71</th>
      <td id="T_7fa37_row70_col0" class="data row70 col0" >LAKEVIEW LOAN SERVICING, LLC</td>
      <td id="T_7fa37_row70_col1" class="data row70 col1" >48</td>
      <td id="T_7fa37_row70_col2" class="data row70 col2" >0.07</td>
      <td id="T_7fa37_row70_col3" class="data row70 col3" >98.87</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row71" class="row_heading level0 row71" >72</th>
      <td id="T_7fa37_row71_col0" class="data row71 col0" >FLEET MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row71_col1" class="data row71 col1" >45</td>
      <td id="T_7fa37_row71_col2" class="data row71 col2" >0.06</td>
      <td id="T_7fa37_row71_col3" class="data row71 col3" >98.93</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row72" class="row_heading level0 row72" >73</th>
      <td id="T_7fa37_row72_col0" class="data row72 col0" >TEXAS CAPITAL BANK, N.A.</td>
      <td id="T_7fa37_row72_col1" class="data row72 col1" >43</td>
      <td id="T_7fa37_row72_col2" class="data row72 col2" >0.06</td>
      <td id="T_7fa37_row72_col3" class="data row72 col3" >98.99</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row73" class="row_heading level0 row73" >74</th>
      <td id="T_7fa37_row73_col0" class="data row73 col0" >TEXAS CAPITAL BANK</td>
      <td id="T_7fa37_row73_col1" class="data row73 col1" >43</td>
      <td id="T_7fa37_row73_col2" class="data row73 col2" >0.06</td>
      <td id="T_7fa37_row73_col3" class="data row73 col3" >99.05</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row74" class="row_heading level0 row74" >75</th>
      <td id="T_7fa37_row74_col0" class="data row74 col0" >USAA FEDERAL SAVINGS BANK</td>
      <td id="T_7fa37_row74_col1" class="data row74 col1" >43</td>
      <td id="T_7fa37_row74_col2" class="data row74 col2" >0.06</td>
      <td id="T_7fa37_row74_col3" class="data row74 col3" >99.11</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row75" class="row_heading level0 row75" >76</th>
      <td id="T_7fa37_row75_col0" class="data row75 col0" >GREENLIGHT FINANCIAL SERVICES</td>
      <td id="T_7fa37_row75_col1" class="data row75 col1" >43</td>
      <td id="T_7fa37_row75_col2" class="data row75 col2" >0.06</td>
      <td id="T_7fa37_row75_col3" class="data row75 col3" >99.17</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row76" class="row_heading level0 row76" >77</th>
      <td id="T_7fa37_row76_col0" class="data row76 col0" >STEARNS LENDING, INC.</td>
      <td id="T_7fa37_row76_col1" class="data row76 col1" >42</td>
      <td id="T_7fa37_row76_col2" class="data row76 col2" >0.06</td>
      <td id="T_7fa37_row76_col3" class="data row76 col3" >99.22</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row77" class="row_heading level0 row77" >78</th>
      <td id="T_7fa37_row77_col0" class="data row77 col0" >NATIONSBANK, N.A.</td>
      <td id="T_7fa37_row77_col1" class="data row77 col1" >40</td>
      <td id="T_7fa37_row77_col2" class="data row77 col2" >0.05</td>
      <td id="T_7fa37_row77_col3" class="data row77 col3" >99.28</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row78" class="row_heading level0 row78" >79</th>
      <td id="T_7fa37_row78_col0" class="data row78 col0" >BRANCH BANKING AND TRUST COMPANY</td>
      <td id="T_7fa37_row78_col1" class="data row78 col1" >35</td>
      <td id="T_7fa37_row78_col2" class="data row78 col2" >0.05</td>
      <td id="T_7fa37_row78_col3" class="data row78 col3" >99.33</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row79" class="row_heading level0 row79" >80</th>
      <td id="T_7fa37_row79_col0" class="data row79 col0" >STONEGATE MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row79_col1" class="data row79 col1" >34</td>
      <td id="T_7fa37_row79_col2" class="data row79 col2" >0.05</td>
      <td id="T_7fa37_row79_col3" class="data row79 col3" >99.37</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row80" class="row_heading level0 row80" >81</th>
      <td id="T_7fa37_row80_col0" class="data row80 col0" >HSBC BANK</td>
      <td id="T_7fa37_row80_col1" class="data row80 col1" >33</td>
      <td id="T_7fa37_row80_col2" class="data row80 col2" >0.05</td>
      <td id="T_7fa37_row80_col3" class="data row80 col3" >99.42</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row81" class="row_heading level0 row81" >82</th>
      <td id="T_7fa37_row81_col0" class="data row81 col0" >GUILD MORTGAGE COMPANY</td>
      <td id="T_7fa37_row81_col1" class="data row81 col1" >32</td>
      <td id="T_7fa37_row81_col2" class="data row81 col2" >0.04</td>
      <td id="T_7fa37_row81_col3" class="data row81 col3" >99.46</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row82" class="row_heading level0 row82" >83</th>
      <td id="T_7fa37_row82_col0" class="data row82 col0" >WACHOVIA BANK, N.A.</td>
      <td id="T_7fa37_row82_col1" class="data row82 col1" >30</td>
      <td id="T_7fa37_row82_col2" class="data row82 col2" >0.04</td>
      <td id="T_7fa37_row82_col3" class="data row82 col3" >99.50</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row83" class="row_heading level0 row83" >84</th>
      <td id="T_7fa37_row83_col0" class="data row83 col0" >FIFTH THIRD BANK, NATIONAL ASSOCIATION</td>
      <td id="T_7fa37_row83_col1" class="data row83 col1" >29</td>
      <td id="T_7fa37_row83_col2" class="data row83 col2" >0.04</td>
      <td id="T_7fa37_row83_col3" class="data row83 col3" >99.54</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row84" class="row_heading level0 row84" >85</th>
      <td id="T_7fa37_row84_col0" class="data row84 col0" >AMERISAVE MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row84_col1" class="data row84 col1" >26</td>
      <td id="T_7fa37_row84_col2" class="data row84 col2" >0.04</td>
      <td id="T_7fa37_row84_col3" class="data row84 col3" >99.58</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row85" class="row_heading level0 row85" >86</th>
      <td id="T_7fa37_row85_col0" class="data row85 col0" >WITMER FUNDING, LLC</td>
      <td id="T_7fa37_row85_col1" class="data row85 col1" >25</td>
      <td id="T_7fa37_row85_col2" class="data row85 col2" >0.03</td>
      <td id="T_7fa37_row85_col3" class="data row85 col3" >99.61</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row86" class="row_heading level0 row86" >87</th>
      <td id="T_7fa37_row86_col0" class="data row86 col0" >CHARTER ONE BANK, N. A.</td>
      <td id="T_7fa37_row86_col1" class="data row86 col1" >25</td>
      <td id="T_7fa37_row86_col2" class="data row86 col2" >0.03</td>
      <td id="T_7fa37_row86_col3" class="data row86 col3" >99.65</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row87" class="row_heading level0 row87" >88</th>
      <td id="T_7fa37_row87_col0" class="data row87 col0" >FIFTH THIRD MORTGAGE COMPANY</td>
      <td id="T_7fa37_row87_col1" class="data row87 col1" >21</td>
      <td id="T_7fa37_row87_col2" class="data row87 col2" >0.03</td>
      <td id="T_7fa37_row87_col3" class="data row87 col3" >99.68</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row88" class="row_heading level0 row88" >89</th>
      <td id="T_7fa37_row88_col0" class="data row88 col0" >COLORADO FEDERAL SAVINGS BANK</td>
      <td id="T_7fa37_row88_col1" class="data row88 col1" >20</td>
      <td id="T_7fa37_row88_col2" class="data row88 col2" >0.03</td>
      <td id="T_7fa37_row88_col3" class="data row88 col3" >99.70</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row89" class="row_heading level0 row89" >90</th>
      <td id="T_7fa37_row89_col0" class="data row89 col0" >STEARNS LENDING, LLC.</td>
      <td id="T_7fa37_row89_col1" class="data row89 col1" >19</td>
      <td id="T_7fa37_row89_col2" class="data row89 col2" >0.03</td>
      <td id="T_7fa37_row89_col3" class="data row89 col3" >99.73</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row90" class="row_heading level0 row90" >91</th>
      <td id="T_7fa37_row90_col0" class="data row90 col0" >FLAGSTAR BANK, NATIONAL ASSOCIATION</td>
      <td id="T_7fa37_row90_col1" class="data row90 col1" >19</td>
      <td id="T_7fa37_row90_col2" class="data row90 col2" >0.03</td>
      <td id="T_7fa37_row90_col3" class="data row90 col3" >99.76</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row91" class="row_heading level0 row91" >92</th>
      <td id="T_7fa37_row91_col0" class="data row91 col0" >CALIBER FUNDING LLC</td>
      <td id="T_7fa37_row91_col1" class="data row91 col1" >17</td>
      <td id="T_7fa37_row91_col2" class="data row91 col2" >0.02</td>
      <td id="T_7fa37_row91_col3" class="data row91 col3" >99.78</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row92" class="row_heading level0 row92" >93</th>
      <td id="T_7fa37_row92_col0" class="data row92 col0" >GUARANTY BANK, SSB</td>
      <td id="T_7fa37_row92_col1" class="data row92 col1" >15</td>
      <td id="T_7fa37_row92_col2" class="data row92 col2" >0.02</td>
      <td id="T_7fa37_row92_col3" class="data row92 col3" >99.80</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row93" class="row_heading level0 row93" >94</th>
      <td id="T_7fa37_row93_col0" class="data row93 col0" >FIRST HORIZON HOME LOAN CORPORATION</td>
      <td id="T_7fa37_row93_col1" class="data row93 col1" >14</td>
      <td id="T_7fa37_row93_col2" class="data row93 col2" >0.02</td>
      <td id="T_7fa37_row93_col3" class="data row93 col3" >99.82</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row94" class="row_heading level0 row94" >95</th>
      <td id="T_7fa37_row94_col0" class="data row94 col0" >DIME SAVINGS BANK OF NEW YORK, FSB</td>
      <td id="T_7fa37_row94_col1" class="data row94 col1" >13</td>
      <td id="T_7fa37_row94_col2" class="data row94 col2" >0.02</td>
      <td id="T_7fa37_row94_col3" class="data row94 col3" >99.84</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row95" class="row_heading level0 row95" >96</th>
      <td id="T_7fa37_row95_col0" class="data row95 col0" >CHICAGO MORTGAGE SOLUTIONS CORP DBA INTERBANK MORTGAGE CO.</td>
      <td id="T_7fa37_row95_col1" class="data row95 col1" >13</td>
      <td id="T_7fa37_row95_col2" class="data row95 col2" >0.02</td>
      <td id="T_7fa37_row95_col3" class="data row95 col3" >99.86</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row96" class="row_heading level0 row96" >97</th>
      <td id="T_7fa37_row96_col0" class="data row96 col0" >MOVEMENT MORTGAGE, LLC</td>
      <td id="T_7fa37_row96_col1" class="data row96 col1" >12</td>
      <td id="T_7fa37_row96_col2" class="data row96 col2" >0.02</td>
      <td id="T_7fa37_row96_col3" class="data row96 col3" >99.87</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row97" class="row_heading level0 row97" >98</th>
      <td id="T_7fa37_row97_col0" class="data row97 col0" >ALLY BANK</td>
      <td id="T_7fa37_row97_col1" class="data row97 col1" >12</td>
      <td id="T_7fa37_row97_col2" class="data row97 col2" >0.02</td>
      <td id="T_7fa37_row97_col3" class="data row97 col3" >99.89</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row98" class="row_heading level0 row98" >99</th>
      <td id="T_7fa37_row98_col0" class="data row98 col0" >IMPAC MORTGAGE CORP.</td>
      <td id="T_7fa37_row98_col1" class="data row98 col1" >12</td>
      <td id="T_7fa37_row98_col2" class="data row98 col2" >0.02</td>
      <td id="T_7fa37_row98_col3" class="data row98 col3" >99.91</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row99" class="row_heading level0 row99" >100</th>
      <td id="T_7fa37_row99_col0" class="data row99 col0" >PLANET HOME LENDING, LLC</td>
      <td id="T_7fa37_row99_col1" class="data row99 col1" >12</td>
      <td id="T_7fa37_row99_col2" class="data row99 col2" >0.02</td>
      <td id="T_7fa37_row99_col3" class="data row99 col3" >99.92</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row100" class="row_heading level0 row100" >101</th>
      <td id="T_7fa37_row100_col0" class="data row100 col0" >NORTHPOINTE BANK</td>
      <td id="T_7fa37_row100_col1" class="data row100 col1" >10</td>
      <td id="T_7fa37_row100_col2" class="data row100 col2" >0.01</td>
      <td id="T_7fa37_row100_col3" class="data row100 col3" >99.94</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row101" class="row_heading level0 row101" >102</th>
      <td id="T_7fa37_row101_col0" class="data row101 col0" >PROSPECT MORTGAGE, LLC</td>
      <td id="T_7fa37_row101_col1" class="data row101 col1" >10</td>
      <td id="T_7fa37_row101_col2" class="data row101 col2" >0.01</td>
      <td id="T_7fa37_row101_col3" class="data row101 col3" >99.95</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row102" class="row_heading level0 row102" >103</th>
      <td id="T_7fa37_row102_col0" class="data row102 col0" >MARINE MIDLAND BANK</td>
      <td id="T_7fa37_row102_col1" class="data row102 col1" >9</td>
      <td id="T_7fa37_row102_col2" class="data row102 col2" >0.01</td>
      <td id="T_7fa37_row102_col3" class="data row102 col3" >99.96</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row103" class="row_heading level0 row103" >104</th>
      <td id="T_7fa37_row103_col0" class="data row103 col0" >SOVEREIGN BANK</td>
      <td id="T_7fa37_row103_col1" class="data row103 col1" >7</td>
      <td id="T_7fa37_row103_col2" class="data row103 col2" >0.01</td>
      <td id="T_7fa37_row103_col3" class="data row103 col3" >99.97</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row104" class="row_heading level0 row104" >105</th>
      <td id="T_7fa37_row104_col0" class="data row104 col0" >PACIFIC UNION FINANCIAL, LLC</td>
      <td id="T_7fa37_row104_col1" class="data row104 col1" >7</td>
      <td id="T_7fa37_row104_col2" class="data row104 col2" >0.01</td>
      <td id="T_7fa37_row104_col3" class="data row104 col3" >99.98</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row105" class="row_heading level0 row105" >106</th>
      <td id="T_7fa37_row105_col0" class="data row105 col0" >DITECH FINANCIAL LLC</td>
      <td id="T_7fa37_row105_col1" class="data row105 col1" >5</td>
      <td id="T_7fa37_row105_col2" class="data row105 col2" >0.01</td>
      <td id="T_7fa37_row105_col3" class="data row105 col3" >99.99</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row106" class="row_heading level0 row106" >107</th>
      <td id="T_7fa37_row106_col0" class="data row106 col0" >TAYLOR, BEAN & WHITAKER MORTGAGE CORPORATION</td>
      <td id="T_7fa37_row106_col1" class="data row106 col1" >5</td>
      <td id="T_7fa37_row106_col2" class="data row106 col2" >0.01</td>
      <td id="T_7fa37_row106_col3" class="data row106 col3" >99.99</td>
    </tr>
    <tr>
      <th id="T_7fa37_level0_row107" class="row_heading level0 row107" >108</th>
      <td id="T_7fa37_row107_col0" class="data row107 col0" >PULTE MORTGAGE LLC</td>
      <td id="T_7fa37_row107_col1" class="data row107 col1" >4</td>
      <td id="T_7fa37_row107_col2" class="data row107 col2" >0.01</td>
      <td id="T_7fa37_row107_col3" class="data row107 col3" >100.00</td>
    </tr>
  </tbody>
</table>




## Question 5

#### Reducing the dataset to include the necessary years


```python
df2 = df1[(df1['oyear'] >= 2012) & (df1['oyear'] <= 2023)]
```

#### Creating the regression Equation


```python
regression_model = sm_ols('Rate ~  UPB + Term + LTV + DTI + FICO + C(oyear)', data=df2).fit()
```

Please note that there is no need to create dummy variables for all the years (-1) in Python. When using the function sm_ols(), if you wrap a variable around C() in the equation you are telling the function to treat this variable as a categorical variable. In this manner, the function will automatically create dummy variables when developing the regression model. Hence in the output shown below, there are coefficients for every single year (Except for 2012). 

### First Way to Print Regression Equation


```python
info_dict={'R-squared' : lambda x: f"{x.rsquared:.2f}",
           'Adj R-squared' : lambda x: f"{x.rsquared_adj:.2f}",
           'No. observations' : lambda x: f"{int(x.nobs):d}"}

# This summary col function combines a bunch of regressions into one nice table
print('='*33)
print("                  y = Rate")
print(summary_col(results=[regression_model], # list the result obj here
                  float_format='%0.6f',
                  stars = True, # stars are easy way to see if anything is statistically significant
                  model_names=['Regression Model'], # these are bad names, lol. Usually, just use the y variable name
                  info_dict=info_dict,
                  regressor_order=[ 'Intercept','UPB','Term','LTV','DTI','FICO',
                                  "C(oyear)[T.2013]", "C(oyear)[T.2014]", "C(oyear)[T.2015]",
                                  "C(oyear)[T.2016]", "C(oyear)[T.2017]", "C(oyear)[T.2018]",
                                  "C(oyear)[T.2019]", "C(oyear)[T.2020]", "C(oyear)[T.2021]",
                                  "C(oyear)[T.2022]", "C(oyear)[T.2023]"]
                  )
     )
```

    =================================
                      y = Rate
    
    =================================
                     Regression Model
    ---------------------------------
    Intercept        3.963274***     
                     (0.057749)      
    UPB              -0.000001***    
                     (0.000000)      
    Term             0.003772***     
                     (0.000045)      
    LTV              0.002183***     
                     (0.000182)      
    DTI              0.003759***     
                     (0.000321)      
    FICO             -0.002098***    
                     (0.000068)      
    C(oyear)[T.2013] 0.188277***     
                     (0.016187)      
    C(oyear)[T.2014] 0.502853***     
                     (0.015619)      
    C(oyear)[T.2015] 0.212522***     
                     (0.015351)      
    C(oyear)[T.2016] 0.031216**      
                     (0.015226)      
    C(oyear)[T.2017] 0.393333***     
                     (0.015249)      
    C(oyear)[T.2018] 0.913512***     
                     (0.015235)      
    C(oyear)[T.2019] 0.438511***     
                     (0.015182)      
    C(oyear)[T.2020] -0.500923***    
                     (0.015103)      
    C(oyear)[T.2021] -0.747499***    
                     (0.015148)      
    C(oyear)[T.2022] 1.263610***     
                     (0.015339)      
    C(oyear)[T.2023] 2.476677***     
                     (0.022475)      
    R-squared        0.686620        
    R-squared Adj.   0.686457        
    R-squared        0.69            
    Adj R-squared    0.69            
    No. observations 30851           
    =================================
    Standard errors in parentheses.
    * p<.1, ** p<.05, ***p<.01


### Second Way to Display Regression Equation


```python
regression_model.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>Rate</td>       <th>  R-squared:         </th> <td>   0.687</td> 
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.686</td> 
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   4222.</td> 
</tr>
<tr>
  <th>Date:</th>             <td>Tue, 05 Dec 2023</td> <th>  Prob (F-statistic):</th>  <td>  0.00</td>  
</tr>
<tr>
  <th>Time:</th>                 <td>22:38:22</td>     <th>  Log-Likelihood:    </th> <td> -23466.</td> 
</tr>
<tr>
  <th>No. Observations:</th>      <td> 30851</td>      <th>  AIC:               </th> <td>4.697e+04</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 30834</td>      <th>  BIC:               </th> <td>4.711e+04</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>    16</td>      <th>                     </th>     <td> </td>    
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>    
</tr>
</table>
<table class="simpletable">
<tr>
          <td></td>            <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th>        <td>    3.9633</td> <td>    0.058</td> <td>   68.630</td> <td> 0.000</td> <td>    3.850</td> <td>    4.076</td>
</tr>
<tr>
  <th>C(oyear)[T.2013]</th> <td>    0.1883</td> <td>    0.016</td> <td>   11.632</td> <td> 0.000</td> <td>    0.157</td> <td>    0.220</td>
</tr>
<tr>
  <th>C(oyear)[T.2014]</th> <td>    0.5029</td> <td>    0.016</td> <td>   32.196</td> <td> 0.000</td> <td>    0.472</td> <td>    0.533</td>
</tr>
<tr>
  <th>C(oyear)[T.2015]</th> <td>    0.2125</td> <td>    0.015</td> <td>   13.844</td> <td> 0.000</td> <td>    0.182</td> <td>    0.243</td>
</tr>
<tr>
  <th>C(oyear)[T.2016]</th> <td>    0.0312</td> <td>    0.015</td> <td>    2.050</td> <td> 0.040</td> <td>    0.001</td> <td>    0.061</td>
</tr>
<tr>
  <th>C(oyear)[T.2017]</th> <td>    0.3933</td> <td>    0.015</td> <td>   25.794</td> <td> 0.000</td> <td>    0.363</td> <td>    0.423</td>
</tr>
<tr>
  <th>C(oyear)[T.2018]</th> <td>    0.9135</td> <td>    0.015</td> <td>   59.962</td> <td> 0.000</td> <td>    0.884</td> <td>    0.943</td>
</tr>
<tr>
  <th>C(oyear)[T.2019]</th> <td>    0.4385</td> <td>    0.015</td> <td>   28.884</td> <td> 0.000</td> <td>    0.409</td> <td>    0.468</td>
</tr>
<tr>
  <th>C(oyear)[T.2020]</th> <td>   -0.5009</td> <td>    0.015</td> <td>  -33.168</td> <td> 0.000</td> <td>   -0.531</td> <td>   -0.471</td>
</tr>
<tr>
  <th>C(oyear)[T.2021]</th> <td>   -0.7475</td> <td>    0.015</td> <td>  -49.347</td> <td> 0.000</td> <td>   -0.777</td> <td>   -0.718</td>
</tr>
<tr>
  <th>C(oyear)[T.2022]</th> <td>    1.2636</td> <td>    0.015</td> <td>   82.381</td> <td> 0.000</td> <td>    1.234</td> <td>    1.294</td>
</tr>
<tr>
  <th>C(oyear)[T.2023]</th> <td>    2.4767</td> <td>    0.022</td> <td>  110.196</td> <td> 0.000</td> <td>    2.433</td> <td>    2.521</td>
</tr>
<tr>
  <th>UPB</th>              <td>-5.431e-07</td> <td> 2.29e-08</td> <td>  -23.735</td> <td> 0.000</td> <td>-5.88e-07</td> <td>-4.98e-07</td>
</tr>
<tr>
  <th>Term</th>             <td>    0.0038</td> <td> 4.45e-05</td> <td>   84.769</td> <td> 0.000</td> <td>    0.004</td> <td>    0.004</td>
</tr>
<tr>
  <th>LTV</th>              <td>    0.0022</td> <td>    0.000</td> <td>   12.019</td> <td> 0.000</td> <td>    0.002</td> <td>    0.003</td>
</tr>
<tr>
  <th>DTI</th>              <td>    0.0038</td> <td>    0.000</td> <td>   11.717</td> <td> 0.000</td> <td>    0.003</td> <td>    0.004</td>
</tr>
<tr>
  <th>FICO</th>             <td>   -0.0021</td> <td> 6.81e-05</td> <td>  -30.805</td> <td> 0.000</td> <td>   -0.002</td> <td>   -0.002</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>2491.237</td> <th>  Durbin-Watson:     </th> <td>   1.992</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>15192.203</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.065</td>  <th>  Prob(JB):          </th> <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 6.435</td>  <th>  Cond. No.          </th> <td>5.72e+06</td> 
</tr>
</table><br/><br/>Notes:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[2] The condition number is large, 5.72e+06. This might indicate that there are<br/>strong multicollinearity or other numerical problems.


