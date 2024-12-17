# 📊 Analysis: The Impact of the Certain Price of Natural Gas Policy on Indonesia’s Economy  

## 📖 **Project Overview**

This project looks at how the **Certain Price of Natural Gas Policy**, introduced by the Indonesian government through **Presidential Decree Number 40/2016** (and later revised by **Presidential Decree Number 121/2020**), has influenced Indonesia’s economy.  

The goal is to understand how allowing industries access to lower natural gas prices has affected the economy, specifically GDP growth, manufacturing, and natural gas production. The idea was to help industries save on production costs and boost economic activity, but did it actually lead to changes in the big picture?

### 🔍 **Research Question**

How has implementing the **Certain Price of Natural Gas Policy** impacted Indonesia’s economy, focusing on GDP growth and economic activity by making natural gas prices cheaper for industries?

### 🛠️ **Objective**

1. Look at the trends in natural gas production, manufacturing output, and GDP over time.
2. See if cheaper natural gas has contributed to economic improvements or changes in production patterns. 
3. Address the limitations of this study compared to the **Indonesian Inter-Regional Static Comparative General Equilibrium (CGE)** model.

### **Methodology**

- **Descriptive Analysis**: Visual trends of GDP, manufacturing output, and natural gas consumption over a timeline.  
- **Regression Analysis**: Statistical modeling to identify patterns and relationships between these variables and the policy’s effects.  
 
 
- **Limitations**:   
  - This analysis oes **NOT** use advanced simulation models due to data and resource constraints. Instead, the findings rely on regression statistics and trend analysis.  


### 🛠️ **Tech Stack**

- **Python**: Primary programming language.  
- **Pandas**: For handling and manipulating data.  
- **Matplotlib/Seaborn**: Visualization tools.  
- **Statsmodels/Scikit-learn**: For regression modeling and statistical analysis.  

### 📂 **Data**

The analysis uses the following datasets:

The following datasets form the foundation of this analysis:  

1. [**GDP Data**](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD?locations=ID): GDP values over the years.  
2. [**Manufacturing Output**](https://www.macrotrends.net/global-metrics/countries/IDN/indonesia/manufacturing-output): Historical manufacturing production values in USD.  
3. [**Natural Gas Production**](https://www.bps.go.id/en/statistics-table/1/MTA5MiMx/produksi-minyak-bumi-dan-gas-alam-1996-2021.html): Monthly production data for natural gas from BPS.

These datasets were cleaned, merged, and analyzed with statistical methods to uncover relationships and trends.

If you want to see the code that I used for data cleaning, you can check it [here](https://github.com/adinramaadin/Python_Project-1_ESDM/blob/main/Python_Code/1_Cleaning.ipynb).

## 📊 **Key Findings**  

### ***Line Trends*** 

![image](https://github.com/user-attachments/assets/c967fd04-9704-49e4-a69c-0a46eff2e655)

Looking at the **Natural Gas Output** trend, we can observe a steady decline since 2015. In contrast, **Manufacturing Output** and **GDP** have shown an increasing trend over the same period. As a result, the gap between these two (manufacturing output and GDP) and natural gas output has widened over time.  

This decline and the concurrent increase in these sectors appear to align with two important policy changes.

1. **2016 - Presidential Decree Number 40/2016**:  
   - Natural gas pricing adjustments were set by the **Minister of Energy and Mineral Resources (ESDM)** in coordination with other relevant ministers.  
   - Focused on economic feasibility for industries using natural gas and ensuring availability.


2. **2020 - Presidential Decree Number 121/2020**:  
   - The **President** directly led the decision-making process, expanding **HGBT (Harga Gas Bumi Tertentu)** eligibility to include: Fertilizer, petrochemical, oleochemical, steel, ceramic, glass, rubber gloves, and **electricity providers for public interest** 
   - Economic focus shifted toward **increasing the added value of industries** by utilizing natural gas.  
   - Pricing mechanisms expanded to include both **contractor prices** and **distribution tariffs**.  
 

The timing of these policies seems to match the downward trend in natural gas output. It might be worth exploring how these decisions impacted production levels.

***We wanted to see if these policies actually influenced Indonesia's natural gas output, manufacturing, and GDP growth.***

## **Statistical Analysis**

Using the **OLS regression model**, I fit the data to determine how these economic variables and the policy introduction relate to natural gas production changes over time. This statistical approach offers insights into whether trends in manufacturing, GDP, and the implementation of the pricing policy impacted the production levels of natural gas.

### ***Code for Initial Regression Model:***

``` py
# Create Post-policy binary column (2016 onward)
normalized_df['post_policy'] = normalized_df['Year'].apply(lambda x: 1 if x >= 2016 else 0)

# Handle Missing values - drop NaNs
normalized_df.dropna(subset=['Manufacture_Output_Billions_of_USD', 'Natural_Gas_Output_MMscf', 'GDP'], inplace=True)

# Model Variables
# GDP as the dependent variable
X = normalized_df[['Manufacture_Output_Billions_of_USD', 'Natural_Gas_Output_MMscf', 'post_policy']]
y = normalized_df['GDP']

# Add intercept for regression
X = sm.add_constant(X)

# Fit the OLS Model
model = sm.OLS(y, X).fit()
print(model.summary())
```

### ***Model Summary Output:***

```
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                    GDP   R-squared:                       0.986
Model:                            OLS   Adj. R-squared:                  0.984
Method:                 Least Squares   F-statistic:                     486.9
Date:                Tue, 17 Dec 2024   Prob (F-statistic):           7.44e-19
Time:                        20:23:44   Log-Likelihood:                 44.550
No. Observations:                  24   AIC:                            -81.10
Df Residuals:                      20   BIC:                            -76.39
Df Model:                           3                                         
Covariance Type:            nonrobust                                         
======================================================================================================
                                         coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------------------------------
const                                 -0.0493      0.030     -1.618      0.121      -0.113       0.014
Manufacture_Output_Billions_of_USD     0.8697      0.036     23.918      0.000       0.794       0.946
Natural_Gas_Output_MMscf               0.0041      0.047      0.088      0.931      -0.093       0.102
post_policy                            0.1106      0.030      3.724      0.001       0.049       0.173
==============================================================================
Omnibus:                        0.301   Durbin-Watson:                   0.747
Prob(Omnibus):                  0.860   Jarque-Bera (JB):                0.458
Skew:                          -0.199   Prob(JB):                        0.795
Kurtosis:                       2.452   Cond. No.                         8.19
==============================================================================
```

### ***Focused Analysis***

To better understand the policy's effect, I performed separate regressions for GDP, manufacturing output, and natural gas output against the `post_policy` variable.

***GDP Analysis***

```py
# Create Post-policy binary column (2016 onward)
merged_df['post_policy'] = merged_df['Year'].apply(lambda x: 1 if x >= 2016 else 0)

# Set up the independent variable (add constant for intercept)
X = sm.add_constant(merged_df['post_policy'])

# Regress GDP on PostPolicy
model_gdp = sm.OLS(merged_df['GDP'], X).fit()
print("\nGDP Model Results:")
print(model_gdp.summary())
```

***Output:***

```
GDP Model Results:
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                    GDP   R-squared:                       0.600
Model:                            OLS   Adj. R-squared:                  0.582
Method:                 Least Squares   F-statistic:                     32.99
Date:                Tue, 17 Dec 2024   Prob (F-statistic):           8.90e-06
Time:                        21:42:31   Log-Likelihood:                -664.10
No. Observations:                  24   AIC:                             1332.
Df Residuals:                      22   BIC:                             1335.
Df Model:                           1                                         
Covariance Type:            nonrobust                                         
===============================================================================
                  coef    std err          t      P>|t|      [0.025      0.975]
-------------------------------------------------------------------------------
const        4.116e+11    6.2e+10      6.639      0.000    2.83e+11     5.4e+11
post_policy  7.121e+11   1.24e+11      5.743      0.000    4.55e+11    9.69e+11
==============================================================================
Omnibus:                        4.084   Durbin-Watson:                   0.343
Prob(Omnibus):                  0.130   Jarque-Bera (JB):                3.446
Skew:                           0.911   Prob(JB):                        0.179
Kurtosis:                       2.642   Cond. No.                         2.48
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
```
***Manufacturing Output:***

```py
# Regress Manufacturing Output on PostPolicy
model_manufacturing = sm.OLS(merged_df['Manufacture_Output_Billions_of_USD'], X).fit()
print("\nManufacturing Output Model Results:")
print(model_manufacturing.summary())
```

***Output:***
```
Manufacturing Output Model Results:
                                    OLS Regression Results                                    
==============================================================================================
Dep. Variable:     Manufacture_Output_Billions_of_USD   R-squared:                       0.505
Model:                                            OLS   Adj. R-squared:                  0.483
Method:                                 Least Squares   F-statistic:                     22.48
Date:                                Tue, 17 Dec 2024   Prob (F-statistic):           9.87e-05
Time:                                        21:43:26   Log-Likelihood:                -128.28
No. Observations:                                  24   AIC:                             260.6
Df Residuals:                                      22   BIC:                             262.9
Df Model:                                           1                                         
Covariance Type:                            nonrobust                                         
===============================================================================
                  coef    std err          t      P>|t|      [0.025      0.975]
-------------------------------------------------------------------------------
const         100.4170     12.485      8.043      0.000      74.525     126.309
post_policy   118.3952     24.970      4.741      0.000      66.610     170.180
==============================================================================
Omnibus:                        2.070   Durbin-Watson:                   0.254
Prob(Omnibus):                  0.355   Jarque-Bera (JB):                1.784
Skew:                           0.572   Prob(JB):                        0.410
Kurtosis:                       2.310   Cond. No.                         2.48
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
```

***Natural Gas Output Analysis:***
```py
# Regress Natural Gas Output on PostPolicy
model_natural_gas = sm.OLS(merged_df['Natural_Gas_Output_MMscf'], X).fit()
print("\nNatural Gas Output Model Results:")
print(model_natural_gas.summary())
```

***Output:***
```
Natural Gas Output Model Results:
                               OLS Regression Results                               
====================================================================================
Dep. Variable:     Natural_Gas_Output_MMscf   R-squared:                       0.245
Model:                                  OLS   Adj. R-squared:                  0.211
Method:                       Least Squares   F-statistic:                     7.138
Date:                      Tue, 17 Dec 2024   Prob (F-statistic):             0.0139
Time:                              21:43:50   Log-Likelihood:                -338.67
No. Observations:                        24   AIC:                             681.3
Df Residuals:                            22   BIC:                             683.7
Df Model:                                 1                                         
Covariance Type:                  nonrobust                                         
===============================================================================
                  coef    std err          t      P>|t|      [0.025      0.975]
-------------------------------------------------------------------------------
const        2.945e+06   8.01e+04     36.776      0.000    2.78e+06    3.11e+06
post_policy -4.279e+05    1.6e+05     -2.672      0.014    -7.6e+05   -9.58e+04
==============================================================================
Omnibus:                        4.273   Durbin-Watson:                   1.791
Prob(Omnibus):                  0.118   Jarque-Bera (JB):                2.632
Skew:                          -0.407   Prob(JB):                        0.268
Kurtosis:                       4.404   Cond. No.                         2.48
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
```
# **Conclusion** 

After analyzing the data, here’s what I found about the impact of the policy introduced in 2016:

### ***Overall Model: How Well It Fits***
The first model looks at how different things—like manufacturing output, natural gas output, and whether we’re in a "post-policy" year—affect GDP. Here’s what stood out:

- The model explains 98.6% of the changes in GDP, which is a really good fit (R-squared value of 0.986).
- Manufacturing output had the biggest impact on GDP and was super consistent in every test (p < 0.001).
- The policy itself (post-2016) also played a role in boosting GDP (p < 0.01).

But natural gas output? Not much of a big deal in the overall picture.

### ***Zooming In: What Happens When We Focus on Each Variable?***

1. GDP Jumped by $712.1 Billion Post-policy: 
    - This represents a major economic boost after the policy changes were implemented.
    - This finding is statistically significant (p < 0.001).

2. Manufacturing output increased by $118.4 billion post-policy:
    - Highlighting a substantial boost driven by structural policy changes (p < 0.001).

3. Natural Gas Production Dropped by 427,900 MMscf Post-policy:
    -This finding is statistically significant (p < 0.014).

### ***What Does This Mean?***
The trends point to a clear trade-off:
1. GDP and Manufacturing Output Show Robust Growth Post-policy:
    - The policy reforms had a direct, positive impact on economic activity, particularly in manufacturing.

2. The decline in natural gas production aligns with trends in supply-side economics. When prices for natural gas are low, producers may cut back on supply to avoid operating at a los (Mason & Roberts, 2018; Islam et al., 2024) 


# Conclusion 2.0

## ***Model Limitations & Introduction to CGE as an Advanced Alternative***

This section provides a summary and analysis of findings from the recent study on the Certain Price of Natural Gas Policy and its effects on the economy. The study focuses on how reducing natural gas prices for industries impacts GDP, household income, labor, sectoral outputs, and commodity prices.

### ***Purpose of This Section***
These limitations affect how deeply this ***project*** could analyze the topic. While the findings might could offer insights, they should be interpreted with these factors in mind.

**Computable General Equilibrium (CGE)** models should be use in the future, as it's a more advanced and valid approach.

### ***Limitations of the Current Project***

1. **Lack of Expertise in CGE Modeling:**  
   The study did not use Computable General Equilibrium (CGE) models due to limited familiarity and expertise. CGE models allow for detailed economic interconnections but were not applied here.

2. **Absence of GAMS Software Utilization:**  
   The General Algebraic Modeling System (GAMS) is essential for conducting CGE analyses. However, resource and accessibility limitations prevented its use in this project.


## ***Background***

### ***Why CGE Matters for Future Research***
**CGE models** are advanced analytical tools used to simulate sectoral interactions, market responses, and the broader economy's reaction to shocks. They are especially helpful because:

1. They model market interconnections influenced by sector shifts and policy changes.
2. They include price variable adjustments and structural changes, improving analysis accuracy.
3. They can analyze forward-looking responses in policies like natural gas price reforms.

### ***Why the Certain Price of Natural Gas Policy Matters***

Natural gas holds strategic importance as both an energy source and a key economic driver. Prior research highlights its role in the Indonesian economy:

- **Subsidies and Market Failures**: Incentives can reduce production costs and market failures by improving affordability and accessibility for industries.
- **Gas Prices & Sectoral Investment**: Changes in gas prices impact industrial competitiveness and investment levels, particularly in gas-dependent sectors.
- **Consumer Impacts**: Rising natural gas prices tend to affect low-income households disproportionately.

### ***Theoretical Basis & Literature Review Insights***

Several studies have explored the relationship, using ***CGE*** model, between gas prices, subsidies, and their economic effects:

1. **Gas price subsidies & market failures:**  
   Subsidies can reduce production costs and correct market inefficiencies. *(Wang & Lin, 2014)*  

2. **Government incentives reduce economic distortions:**  
   These incentives lead to better market outcomes by encouraging affordability and improving competitiveness. *(Fattouh & El-Katiri, 2012; Lin & Li, 2021)*  

3. **Gas prices impact investment decisions:**  
   Changes in natural gas prices influence how much industries invest. *(Goncharuk, 2015)*  

4. **Gas subsidies impact economic growth:**  
   Subsidies can lead to expanded production and higher economic growth. *(Ambya et al., 2020)*  

5. **Rising gas prices can lower affordability for households:**  
   This reduces economic efficiency and access to energy. *(Orlov, 2015)*  

6. **Price increases affect environmental factors and consumer prices:**  
   He & Lin (2017) found that gas price increases can reduce carbon emissions, raise CPI, and lower GDP.  

7. **Macro-level benefits from gas subsidies:**  
   Nugroho & Amir (2018) discovered that, on a macroeconomic scale, providing incentives in the form of lower natural gas prices could raise GDP by 0.12% to 0.13%.  

8. **Gas price reductions & broader economic effects:**  
   Prasetyawati & Djoni Hartono (2023) highlighted that reductions in gas prices positively impact macroeconomic indicators like GDP. They emphasized that compensation policies must be included in economic models to evaluate long-term effects more accurately. Future studies employing CGE models and dynamic simulation methods could provide a deeper understanding of these effects.



# 📌 ***Closing Statement***

This analysis provides insights into how the Certain Price of Natural Gas Policy, introduced through Presidential Decree Number 40/2016 and revised by Presidential Decree Number 121/2020, has influenced Indonesia's economic activity. The statistical analysis using OLS regression revealed that the policy's implementation has a significant positive relationship with GDP and manufacturing output, indicating that subsidizing natural gas prices has supported industrial production and economic growth.

However, the results also suggest that natural gas production has faced a downward trend, potentially impacted by these pricing policies and shifting industrial needs. While manufacturing and GDP growth showed promising patterns, the relatively limited response in natural gas output indicates that future strategies should align production incentives with policy objectives.

This study highlights both the opportunities and challenges associated with policy interventions and their complex effects on resource markets and industrial economies. Moving forward, a more comprehensive approach incorporating simulation models or cross-sectoral analysis could enhance these findings.

Thank you for engaging with this project—it's been a meaningful exploration into Indonesia’s energy policy and economic development.

## REFERENCES

[Prasetyawati, I., & Hartono, D. The Impact of Certain Price of Natural Gas Policy on Indonesia’s Economy. Jurnal Ekonomi Pembangunan: Kajian Masalah Ekonomi dan Pembangunan, 24(1), 112-128.](https://journals.ums.ac.id/index.php/JEP/article/view/21544)

[Islam, M. S., Ahmed, F., Islam, M. M., Rehman, A. U., & Alam, M. F. (2024). The Impact of Oil Price Shocks on Oil and Gas Production Amidst Geopolitical Risk in OPEC: Insights from Method of Moments Quantile Regression. Journal of the Knowledge Economy, 1-30.](https://link.springer.com/article/10.1007/s13132-024-02296-y)

[Mason, C. F., & Roberts, G. (2018). Price elasticity of supply and productivity: an analysis of natural gas wells in Wyoming. The Energy Journal, 39(1_suppl), 79-100.](https://journals.sagepub.com/doi/abs/10.5547/01956574.39.SI1.cmas)
