#  House Price Prediction with Linear Regression  
### A Methodological Comparison of Numeric vs Categorical Treatment of Discrete Variables

## 📌 Project Overview

This project focuses on predicting house prices using **Linear Regression**, while simultaneously addressing an important methodological question in regression modeling:

> **How should discrete numerical variables (such as the number of bedrooms, bathrooms, stories, and parking spaces) be treated — as numeric or categorical features?**

Rather than approaching this task as a standard house price prediction problem, the project explicitly compares two different feature engineering strategies and evaluates their impact on model performance, interpretability, and generalization.

The study emphasizes **methodological soundness**, **model diagnostics**, and **interpretability**, making it particularly suitable for academic and data science portfolio purposes.

---

## 🎯 Objectives

The main objectives of this project are:

- To perform a thorough **Exploratory Data Analysis (EDA)** to understand the structure and characteristics of the housing dataset
- To investigate relationships between house prices and both numerical and categorical features
- To design and compare two different feature engineering approaches:
  - Treating discrete numerical variables as **continuous (numeric)**
  - Treating discrete numerical variables as **categorical (one-hot encoded)**
- To train and evaluate **Linear Regression** models under both assumptions
- To analyze model coefficients, residuals, and overfitting behavior
- To draw clear methodological conclusions supported by empirical evidence

---

## 📊 Exploratory Data Analysis (EDA)

Exploratory Data Analysis was conducted to understand the distribution of house prices, examine relationships between numerical features and the target variable, and identify potential modeling challenges such as skewness, non-linearity, and multicollinearity.

---

### 📌 Distribution of House Prices

<img width="686" height="470" alt="download" src="https://github.com/user-attachments/assets/20d83fdb-f123-4bfc-bf8f-a9b2f07d0f29" />


The distribution of the target variable (`price`) exhibits a **right-skewed** pattern.

- Most properties are concentrated in the lower-to-mid price range.
- A small number of high-priced houses form a long right tail.
- This skewness suggests that extreme values (luxury properties) may have a disproportionate impact on model estimation.

**Implication:**  
Right-skewed price distributions are common in real estate data and may lead to heteroskedasticity in regression models, particularly for high-end properties.

---

### 📌 Relationship Between Area and Price

<img width="613" height="470" alt="download-1" src="https://github.com/user-attachments/assets/e14d297d-816c-4c5d-9634-2c74ca46c3df" />


A scatter plot of **price vs. area** reveals a **positive but noisy relationship**:

- Larger houses tend to have higher prices.
- However, there is considerable variability, especially at larger areas.
- The spread of prices increases with area, indicating non-constant variance.

This suggests that while `area` is a strong predictor, it does not fully explain price variability on its own.

---


### 📌 Correlation Analysis of Numeric Features

<img width="673" height="490" alt="download-2" src="https://github.com/user-attachments/assets/8e28c795-212c-4742-81d4-504a11159593" />

A correlation matrix was computed for the numeric variables:

- `area` shows the strongest correlation with `price`.
- `bathrooms` and `stories` also exhibit moderate positive correlations.
- `bedrooms` and `parking` display weaker but still positive relationships.
- No extreme multicollinearity is observed among numeric predictors.

**Implication:**  
These results suggest that numeric features contribute complementary information and can be safely included together in a linear regression framework.

---

### 📌 Log-Transformed Price vs. Area

<img width="622" height="470" alt="download-3" src="https://github.com/user-attachments/assets/686898cc-32c8-400c-8a4a-6824ead776ea" />

To better examine the relationship between size and price, a smoothed line plot of **log(price) vs. area** was analyzed.

- The relationship becomes more linear after log transformation.
- A moderate positive correlation is observed (**corr ≈ 0.54**).
- The log scale reduces the impact of extreme price values and highlights the underlying trend more clearly.

**Implication:**  
This supports the use of linear models while suggesting that transformations (e.g., `log(price)`) may help mitigate heteroskedasticity.

---

### 🧠 Key Insights from EDA

- House prices are **right-skewed**, with increasing variance at higher price levels.
- `area` is a primary driver of price, but the relationship is noisy and variance increases for larger houses.
- `log(price)` helps reveal a clearer linear trend with `area` and reduces the influence of extreme values.
- Numeric predictors show moderate correlations with `price` and no severe multicollinearity.

---

## 📐 Modeling and Evaluation — Model A (Numeric Assumption)

In this section, a **Linear Regression model** is trained under the assumption that discrete numerical variables (such as bedrooms, bathrooms, stories, and parking) are treated as **continuous numeric features**.

---

### ⚙️ Model Configuration

- Algorithm: **Linear Regression**
- Numeric features (scaled):
  - `area`, `bedrooms`, `bathrooms`, `stories`, `parking`
- Categorical features (one-hot encoded):
  - `mainroad`, `guestroom`, `basement`, `hotwaterheating`
  - `airconditioning`, `prefarea`, `furnishingstatus`
- Evaluation metrics:
  - **R²**
  - **RMSE**
  - **MAE**

---

### 📊 Model Performance

<img width="528" height="65" alt="Ekran Resmi 2025-12-14 22 56 36" src="https://github.com/user-attachments/assets/9874404d-bbf6-4499-a530-d0802ab97474" />

The Linear Regression model achieves the following results on the test set:

- **R²:** 0.65  
- **RMSE:** ~1.32 × 10⁶  
- **MAE:** ~9.7 × 10⁵  

These results indicate that the model explains approximately **65% of the variance** in house prices, which is a reasonable performance level for a linear baseline model applied to real estate data.

---

### 📉 Residual Analysis

<img width="612" height="470" alt="download" src="https://github.com/user-attachments/assets/88b96200-4185-4867-9fbb-919329490b10" />

The residual plot provides insight into the model’s error structure:

- Residuals are generally centered around zero, suggesting no systematic over- or under-prediction.
- However, the spread of residuals increases for higher predicted prices.
- This pattern indicates **heteroskedasticity**, meaning prediction errors grow for high-priced properties.

**Interpretation:**

> The residuals are generally centered around zero, indicating that the model does not suffer from systematic over- or under-prediction. However, the spread of residuals increases for higher predicted prices, suggesting heteroskedasticity and reduced predictive accuracy for high-end properties.

---

### 📌 Coefficient Analysis and Feature Importance

<img width="500" height="409" alt="Ekran Resmi 2025-12-14 22 56 57" src="https://github.com/user-attachments/assets/325e07a9-1d79-4da6-b49e-0e4d1d7d6c90" />

An examination of the learned coefficients reveals several key insights:

- **Air conditioning**, **hot water heating**, and **preferred area** have the strongest positive associations with house prices.
- Structural features such as `bathrooms`, `area`, and `stories` also contribute positively.
- Negative coefficients for unfurnished properties indicate a price discount relative to furnished homes.

Since numeric features were standardized, coefficient magnitudes should be interpreted **relatively rather than as absolute monetary effects**. Dummy variables, on the other hand, represent differences relative to their reference categories.

---

## 📐 Modeling and Evaluation — Model B (Categorical Assumption)

In this section, a **Linear Regression model** is trained under the assumption that discrete numerical variables (such as bedrooms, bathrooms, stories, and parking) are treated as **categorical features** and encoded using one-hot encoding.

---

### ⚙️ Model Configuration

- Algorithm: **Linear Regression**
- Numeric features (scaled):
  - `area`
- Categorical features (one-hot encoded):
  - Original categorical variables:
    - `mainroad`, `guestroom`, `basement`, `hotwaterheating`
    - `airconditioning`, `prefarea`, `furnishingstatus`
  - Discrete numerical variables treated as categorical:
    - `bedrooms`, `bathrooms`, `stories`, `parking`
- Evaluation metrics:
  - **R²**
  - **RMSE**
  - **MAE**

---

### 📊 Model Performance

<img width="532" height="75" alt="Ekran Resmi 2025-12-14 23 01 22" src="https://github.com/user-attachments/assets/16927c5a-c3ad-48ca-84d9-e02039d35297" />

The Linear Regression model under the categorical assumption achieves the following results:

- **R²:** 0.65  
- **RMSE:** ~1.33 × 10⁶  
- **MAE:** ~9.5 × 10⁵  

At first glance, these metrics appear **very similar** to those obtained under the numeric assumption (Model A), suggesting comparable predictive accuracy.

---

### 📉 Residual Analysis

<img width="612" height="470" alt="download" src="https://github.com/user-attachments/assets/943964d5-3f06-454f-89f5-dd3c8184f58e" />

The residual plot for Model B shows:

- Residuals generally centered around zero
- No clear systematic bias in prediction direction
- A wide spread of residuals, particularly for higher predicted prices

Although the overall residual pattern resembles that of Model A, the dispersion remains substantial, especially for high-priced properties.

---

### 📌 Coefficient Analysis and Model Stability

<img width="460" height="461" alt="Ekran Resmi 2025-12-14 23 01 36" src="https://github.com/user-attachments/assets/3e177407-9af9-4cea-8cba-80788ce2a99e" />

An inspection of the learned coefficients reveals several critical issues:

- Extremely large coefficients for specific dummy variables (e.g., high bathroom or bedroom counts)
- Coefficients that exceed the scale of the target variable
- Non-monotonic and fragmented effects across ordinal levels

These coefficients **cannot be interpreted as real monetary effects**. Instead, they indicate that the model is fitting noise and rare categories rather than learning stable, generalizable relationships.

---


## 📌 Overall Evaluation

This study compared two alternative feature engineering strategies within a Linear Regression framework to understand how the treatment of discrete numerical variables affects model behavior and interpretability.

Although **Model A (numeric assumption)** and **Model B (categorical assumption)** achieve very similar performance metrics in terms of R², RMSE, and MAE, a deeper inspection reveals important qualitative differences.

Model A, where discrete numerical variables are treated as continuous features, produces **stable, smooth, and interpretable coefficient estimates** that align well with domain knowledge. Structural features such as area, number of bathrooms, and stories exhibit consistent and reasonable effects, while categorical amenities (e.g., air conditioning, preferred area) show meaningful price associations.

In contrast, Model B, which treats discrete numerical variables as categorical, introduces a large number of dummy variables and results in **highly fragmented and inflated coefficient estimates**, especially for rare categories. While predictive accuracy appears comparable on the surface, the learned relationships are less stable and considerably harder to interpret in a meaningful way.

Overall, this comparison demonstrates that **similar predictive performance does not guarantee methodological soundness**. The numeric treatment of discrete numerical variables leads to a more robust, interpretable, and theoretically consistent model, making Model A the preferred approach for this task.

---

## 🚀 Potential Improvements and Future Work

Several directions can be explored to further improve the modeling approach:

- **Target Transformation:**  
  Applying a log transformation to the target variable (`log(price)`) may help reduce heteroskedasticity and improve performance for high-end properties.

- **Regularized Regression Models:**  
  Techniques such as Ridge or Lasso regression can be used to stabilize coefficient estimates and reduce sensitivity to correlated predictors.

- **Feature Interaction Terms:**  
  Introducing interaction terms (e.g., area × number of bathrooms) may provide additional explanatory power while remaining within a linear framework.

- **Cross-Validation:**  
  Using cross-validation instead of a single train-test split would provide a more reliable estimate of generalization performance.

These extensions could further enhance predictive accuracy while preserving interpretability, depending on the goals and scope of the analysis.

---

### 👤 Author

**Yiğit Can Kınalı**  
Industrial Engineering  
Machine Learning & Data Science Enthusiast
