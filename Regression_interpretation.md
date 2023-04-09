---
layout: wide_default
---  

# THIS FILE IS IN THE HANDOUTS FOLDER. COPY IT INTO YOUR CLASS NOTES

- [**Read the chapter on the website!**](https://ledatascifi.github.io/ledatascifi-2023/content/05/02_reg.html) It contains a lot of extra information we won't cover in class extensively.
- After reading that, I recommend [this webpage as a complimentary place to get additional intuition.](https://aeturrell.github.io/coding-for-economists/econmt-regression.html)

## Today

[Finish picking teams and declare initial project interests in the project sheet](https://docs.google.com/spreadsheets/d/1kRbuRKfKh9lCdoVBGLxSbDTIRBEfnV7Y8AcP-hZbmTw/edit#gid=1508330834)


# Today is mostly about INTERPRETING COEFFICIENTS (6.4 in the book)

1. 25 min reading groups: Talk/read through two regression pages (6.3 and 6.4) 
    - Assemble your own notes. Perhaps in the "Module 4 notes" file, but you can do this in any file you want.
    - After class, each group will email their notes to the TA/me for participation. (Effort grading.)
1. 10 min: class builds joint "big takeaways and nuanced observations" 
1. 5 min: Interpret models 1-2 as class as practice. 
1. 20 min reading groups: Work through remaining problems below.
1. 10 min: wrap up  

---


```python
import pandas as pd
from statsmodels.formula.api import ols as sm_ols
import numpy as np
import seaborn as sns
from statsmodels.iolib.summary2 import summary_col # nicer tables

```


```python
url = 'https://github.com/LeDataSciFi/ledatascifi-2023/blob/main/data/Fannie_Mae_Plus_Data.gzip?raw=true'
fannie_mae = pd.read_csv(url,compression='gzip') 
```

## Clean the data and create variables you want


```python
fannie_mae = (fannie_mae
                  # create variables
                  .assign(l_credscore = np.log(fannie_mae['Borrower_Credit_Score_at_Origination']),
                          l_LTV = np.log(fannie_mae['Original_LTV_(OLTV)']),
                          l_int = np.log(fannie_mae['Original_Interest_Rate']),
                          Origination_Date = lambda x: pd.to_datetime(x['Origination_Date']),
                          Origination_Year = lambda x: x['Origination_Date'].dt.year,
                          const = 1
                         )
                  .rename(columns={'Original_Interest_Rate':'int'}) # shorter name will help the table formatting
             )

# create a categorical credit bin var with "pd.cut()"
fannie_mae['creditbins']= pd.cut(fannie_mae['Co-borrower_credit_score_at_origination'],
                                 [0,579,669,739,799,850],
                                 labels=['Very Poor','Fair','Good','Very Good','Exceptional'])

```

## Statsmodels

As before, the psuedocode:
```python
model = sm_ols(<formula>, data=<dataframe>)
result=model.fit()

# you use result to print summary, get predicted values (.predict) or residuals (.resid)
```

Now, let's save each regression's result with a different name, and below this, output them all in one nice table:


```python
# one var: 'y ~ x' means fit y = a + b*X

reg1 = sm_ols('int ~  Borrower_Credit_Score_at_Origination ', data=fannie_mae).fit()

reg1b= sm_ols('int ~  l_credscore  ',  data=fannie_mae).fit()

reg1c= sm_ols('l_int ~  Borrower_Credit_Score_at_Origination  ',  data=fannie_mae).fit()

reg1d= sm_ols('l_int ~  l_credscore  ',  data=fannie_mae).fit()

# multiple variables: just add them to the formula
# 'y ~ x1 + x2' means fit y = a + b*x1 + c*x2
reg2 = sm_ols('int ~  l_credscore + l_LTV ',  data=fannie_mae).fit()

# interaction terms: Just use *
# Note: always include each variable separately too! (not just x1*x2, but x1+x2+x1*x2)
reg3 = sm_ols('int ~  l_credscore + l_LTV + l_credscore*l_LTV',  data=fannie_mae).fit()
      
# categorical dummies: C() 
reg4 = sm_ols('int ~  C(creditbins)  ',  data=fannie_mae).fit()

reg5 = sm_ols('int ~  C(creditbins)  -1', data=fannie_mae).fit()

```

Ok, time to output them:


```python
# now I'll format an output table
# I'd like to include extra info in the table (not just coefficients)
info_dict={'R-squared' : lambda x: f"{x.rsquared:.2f}",
           'Adj R-squared' : lambda x: f"{x.rsquared_adj:.2f}",
           'No. observations' : lambda x: f"{int(x.nobs):d}"}

# q4b1 and q4b2 name the dummies differently in the table, so this is a silly fix
reg4.model.exog_names[1:] = reg5.model.exog_names[1:]  

# This summary col function combines a bunch of regressions into one nice table
print('='*108)
print('                  y = interest rate if not specified, log(interest rate else)')
print(summary_col(results=[reg1,reg1b,reg1c,reg1d,reg2,reg3,reg4,reg5], # list the result obj here
                  float_format='%0.4f',
                  stars = True, # stars are easy way to see if anything is statistically significant
                  model_names=['1','2',' 3 (log)','4 (log)','5','6','7','8'], # these are bad names, lol. Usually, just use the y variable name
                  info_dict=info_dict,
                  regressor_order=[ 'Intercept','Borrower_Credit_Score_at_Origination','l_credscore','l_LTV','l_credscore:l_LTV',
                                  'C(creditbins)[Very Poor]','C(creditbins)[Fair]','C(creditbins)[Good]','C(creditbins)[Vrey Good]','C(creditbins)[Exceptional]']
                  )
     )
```

    ============================================================================================================
                      y = interest rate if not specified, log(interest rate else)
    
    ============================================================================================================================
                                             1          2        3 (log)   4 (log)       5           6          7          8    
    ----------------------------------------------------------------------------------------------------------------------------
    Intercept                            11.5819*** 45.3715*** 2.8713***  9.4997***  44.1324*** -16.8119*** 6.6489***           
                                         (0.0457)   (0.2907)   (0.0090)   (0.0571)   (0.3024)   (4.1106)    (0.0812)            
    Borrower_Credit_Score_at_Origination -0.0086***            -0.0017***                                                       
                                         (0.0001)              (0.0000)                                                         
    l_credscore                                     -6.0750***            -1.1920*** -5.9859*** 3.2155***                       
                                                    (0.0440)              (0.0086)   (0.0444)   (0.6205)                        
    l_LTV                                                                            0.1546***  14.6120***                      
                                                                                     (0.0105)   (0.9725)                        
    l_credscore:l_LTV                                                                           -2.1830***                      
                                                                                                (0.1468)                        
    C(creditbins)[Very Poor]                                                                                           6.6489***
                                                                                                                       (0.0812) 
    C(creditbins)[Fair]                                                                                     -0.6281*** 6.0207***
                                                                                                            (0.0829)   (0.0166) 
    C(creditbins)[Good]                                                                                     -1.1682*** 5.4807***
                                                                                                            (0.0817)   (0.0093) 
    C(creditbins)[Exceptional]                                                                              -2.2455*** 4.4034***
                                                                                                            (0.0821)   (0.0120) 
    C(creditbins)[Very Good]                                                                                -1.6461*** 5.0028***
                                                                                                            (0.0814)   (0.0066) 
    R-squared                            0.1259     0.1242     0.1261     0.1240     0.1256     0.1270      0.1138     0.1138   
    R-squared Adj.                       0.1259     0.1241     0.1261     0.1240     0.1256     0.1270      0.1138     0.1138   
    R-squared                            0.13       0.12       0.13       0.12       0.13       0.13        0.11       0.11     
    Adj R-squared                        0.13       0.12       0.13       0.12       0.13       0.13        0.11       0.11     
    No. observations                     134481     134481     134481     134481     134481     134481      67366      67366    
    ============================================================================================================================
    Standard errors in parentheses.
    * p<.1, ** p<.05, ***p<.01


Guidelines:
- Intercept: $E(y|X=0)$, the value of y when x is 0
- Interpreting coefs on X / Beta:
    - A 1 unit increase in X is associated with a B_i change in y, holding all other X constants.
    - Fill in the blanks but i t depends on what X_i is, and f(y)
    - If X is continuous
        - Is x logged and/or y logged?
        - Copy table from textbook here
    - If X is binary
        - beta compares jump from false to true, whatever that means for that variable
        - copy the table here
    - If X is categorical
        - The regression will include n-1 number of categories., assuming n is the number of categories.
        - Beta compares jump from the omitted category to the given level 

# Today. Work in groups. Refer to the lectures. 

You might need to print out a few individual regressions with more decimals.

1. Interpret coefs in model 1-4
    - Model 1: If credit score increases by 1, then interest rate observed on the loan will decrease by 0.86 b.p. An interest rate of 11.5 p.p if credit score is 0. If x is 700, y will be 5.5957. If we go from 700 to 707, the interest rate will fall by approx -0.0607 pp
    - Model 2: If credit score increases by 1% then interest rate will decrease by -0.0607 p.p (6.07 bps) in interest rate. If we go from 700 to 707 (this is a 1% increase) then interest rate fall will be of -0.0607 pp.
    - Model 3: If credit score increases by 1 unit then interest rate will decrease proportionally by 0.17% (17 bps). If x is 700 then y will be 5.5957. If we go from 700 to 707, the interest rate will fall by approx -0.0607 pp.
    - Model 4: A 1% increase in credit score will result in a proportional decrease of 1.1920%.  If x is 700 then y will be 5.5957. If we go from 700 to 707, the interest rate will fall by approx -0.0607 pp.
    - Model 5: A 1% increase in credit score is associated with a decrease of 5.59 bps in interest rate holding l_LTV constant. If we go from 700 to 707, the interest rate will fall by approx -0.0607 pp.
    - Model 6: How the slope of a curve is dternmined by some other variable 
    - Model 7: For example the Fair category coefficient is saying that if you have a credit score that could be said to be Fair, then the reduction in interest rate will be of -0.6281 p.p. than if your credit score could be said to be Very Poor.
    - Model 8: No intercept is present
1. Interpret coefs in model 5
1. Interpret coefs in model 6 (and visually?)
1. Interpret coefs in model 7 (and visually? + comp to table)
1. Interpret coefs in model 8 (and visually? + comp to table)
1. Add l_LTV  to Model 8 and interpret (and visually?)


```python
np.mean(fannie_mae["l_LTV"])
```




    4.207507631459906



Not just look at the p-values, whihc tell us about stat significance, we have to look at economic significance. divide the cont. var by the std. Compare now this coef with the mean of the y var or the std of y. 
