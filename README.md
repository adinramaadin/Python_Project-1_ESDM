# 📊 Analysis: The Impact of the Certain Price of Natural Gas Policy on Indonesia’s Economy  

## 📖 **Project Overview**

This project analyzes the economic impact of the **Certain Price of Natural Gas Policy**, introduced by the Indonesian government through **Presidential Decree Number 40/2016** and revised by **Presidential Decree Number 121/2020**, on Indonesia's economy.

The policy allows industries to access natural gas at lower prices to reduce production costs, increase production capacity, and drive economic growth. This analysis aims to determine whether the intervention has measurable effects on Indonesia’s GDP and production patterns.

### 🔍 **Research Question**

How has the **Certain Price of Natural Gas Policy** affected Indonesia's economy, particularly GDP growth and economic activity, through the reduction of natural gas prices in key industrial sectors?

### 🛠️ **Objective**

1. Analyze natural gas production, manufacturing output, and related GDP trends quantitatively.  
2. Establish if natural gas price reduction has contributed to GDP changes and economic improvements.  
3. Address the limitations of this study compared to the **Indonesian Inter-Regional Static Comparative General Equilibrium (CGE)** model.

### 🏆 **Methodology**

- **Descriptive Analysis**: Visualization of trends in GDP, natural gas consumption, and production over time.  
- **Regression Analysis**: Correlation and regression modeling to identify relationships between natural gas policy implementation and GDP changes.  
- **Limitations**:  
  - The study does **NOT** use advanced GAMS-based models due to data and model availability.  
  - The analysis will rely only on regression and descriptive statistics.

### 🛠️ **Tech Stack**

- **Python** for analysis
- **Pandas**: Data manipulation
- **Matplotlib/Seaborn**: Visualization
- **Statsmodels/Scikit-learn**: Regression analysis  

### 📂 **Data**

The analysis uses the following datasets:

1. **GDP Data**: GDP values from [here](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD?locations=ID]).  
2. **Manufacturing Output**: Historical levels of manufacturing production in USD (here)[https://www.macrotrends.net/global-metrics/countries/IDN/indonesia/manufacturing-output#:~:text=Indonesia%20manufacturing%20output%20for%202022%20was%20%24241.87B%2C%20a,2020%20was%20%24210.40B%2C%20a%204.58%25%20decline%20from%202019.].  
3. **Natural Gas Production**: Natural gas output in MMscf over time from (BPS)[https://www.bps.go.id/en/statistics-table/1/MTA5MiMx/produksi-minyak-bumi-dan-gas-alam-1996-2021.html].  

These datasets will be preprocessed, merged, and analyzed using statistical models to explore correlations and trends over time.
(which if u want to see go here)

## Analysis

![output](https://github.com/user-attachments/assets/05c99ac4-1e9c-49ff-80f1-f01db8811c32)

Looking at the Natural Gas Output trend, we can see that it has been steadily declining over time. This drop appears to align with two important policy changes:

1. **2016 - Presidential Decree Number 40/2016**: This policy focused on implementing certain natural gas pricing for industrial use to stabilize production costs.  
2. **2020 - Presidential Decree Number 121/2020**: This revision introduced sector-specific price adjustments to better align with market demands and economic recovery.

The timing of these policies seems to match the downward trend in natural gas output. It might be worth exploring how these decisions impacted production levels.

so here's what Using the **OLS regression model**, I fit the data to determine how these economic variables and the policy introduction relate to natural gas production changes over time. This statistical approach offers insights into whether trends in manufacturing, GDP, and the implementation of the pricing policy impacted the production levels of natural gas.

```
# Create Post-policy binary column (2016 onward)
normalized_df['post_policy'] = normalized_df['Year'].apply(lambda x: 1 if x >= 2016 else 0)

# Handle Missing values - drop NaNs
normalized_df.dropna(subset=['Natural_Gas_Output_MMscf'], inplace=True)

# Model Variables
# Natural Gas Output as the dependent variable
X = normalized_df[['post_policy', 'Manufacture_Output_Billions_of_USD', 'GDP']]  # Select relevant predictors
y = normalized_df['Natural_Gas_Output_MMscf']

# Add intercept for regression
X = sm.add_constant(X)

# Fit the OLS Model
model = sm.OLS(y, X).fit()
print(model.summary())
```
```
 OLS Regression Results                               
====================================================================================
Dep. Variable:     Natural_Gas_Output_MMscf   R-squared:                       0.246
Model:                                  OLS   Adj. R-squared:                  0.133
Method:                       Least Squares   F-statistic:                     2.174
Date:                      Tue, 17 Dec 2024   Prob (F-statistic):              0.123
Time:                              20:23:57   Log-Likelihood:                 7.0196
No. Observations:                        24   AIC:                            -6.039
Df Residuals:                            20   BIC:                            -1.327
Df Model:                                 3                                         
Covariance Type:                  nonrobust                                         
======================================================================================================
                                         coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------------------------------
const                                  0.5577      0.092      6.080      0.000       0.366       0.749
post_policy                           -0.2362      0.177     -1.335      0.197      -0.605       0.133
Manufacture_Output_Billions_of_USD    -0.1038      0.945     -0.110      0.914      -2.074       1.867
GDP                                    0.0943      1.068      0.088      0.931      -2.133       2.322
==============================================================================
Omnibus:                        4.487   Durbin-Watson:                   1.797
Prob(Omnibus):                  0.106   Jarque-Bera (JB):                2.801
Skew:                          -0.449   Prob(JB):                        0.246
Kurtosis:                       4.413   Cond. No.                         44.3
==============================================================================
```
Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
![image](https://github.com/user-attachments/assets/78e0a448-6075-4be8-84df-f802935951f4)


![image](https://github.com/user-attachments/assets/2afe060b-9c06-47f9-a1f5-a77395b1a03a)

![image](https://github.com/user-attachments/assets/90ca8820-8d16-47e3-a169-efb14a342655)
