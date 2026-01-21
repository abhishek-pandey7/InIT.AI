# Life Expectancy Analysis & Modeling

## Dataset Source

The dataset used in this project is the WHO Life Expectancy Dataset, publicly available on Kaggle.

**Source:**
World Health Organization (WHO) – [Life Expectancy Dataset (Kaggle)](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who)

The dataset contains country-level health, economic, and demographic indicators recorded over multiple years, along with corresponding life expectancy values.

## Problem Statement

The goals of this project are to:

- **Analyze** factors affecting life expectancy across countries.
- **Build** a supervised regression model from scratch and compare with baseline model.
- **Apply** unsupervised learning (K-Means + PCA) to discover hidden structures.
- **Reflect** on how unsupervised insights can improve supervised modeling.

## Project Workflow

### Part 1: Exploratory Data Analysis (EDA)

- Checked dataset shape, data types and missing values.
- Analyzed the distribution of the target variable (**Life Expectancy**).
- Visualized correlations using a heatmap.

#### Key observations:

**Strong positive correlation with life expectancy:**

- Schooling (0.75)
- Income composition of resources (0.72)
- BMI (0.57)
- GDP (0.46)

**Strong negative correlation:**

- Adult Mortality (-0.70)
- HIV/AIDS (-0.56)
- Thinness indicators (~ -0.48)

_Detected outliers below 45 years, which negatively impact linear regression models._

---

### Part 2: Data Preprocessing

#### Handling Missing Values

- Rows with missing target values (Life Expectancy) were dropped (only 0.34% of data).
- Numerical features were imputed using **country-wise means** to preserve each country’s profile.
- If a country had all values missing for a feature, the global mean was used.
- Population was forward/backward filled within countries and later dropped due to very low correlation (0.02).

#### Encoding

- **Status** encoded as binary: Developing → 0, Developed → 1.
- **Country** column dropped to avoid high dimensionality from one-hot encoding (182+ extra features).

#### Scaling

- **RobustScaler** used for supervised learning due to presence of outliers.
- **Train–test split** performed before scaling to avoid data leakage.

---

### Part 3: Supervised Learning

#### Models Implemented

1. **Linear Regression from scratch** using Gradient Descent.
2. **Baseline model** using `sklearn.linear_model.LinearRegression`.

#### Evaluation Metric

- **Mean Squared Error (MSE)**

#### Observations

- Linear regression works well for near-linear features (Schooling, Income, BMI).
- Performs poorly on non-linear relationships (e.g., Measles).
- Multicollinearity affects how we interpret coefficients but does not prevent convergence.

#### Effect of Algorithm Assumptions

- Linear regression assumes linear relationships between predictors and target.
- Works well for socio-economic variables but fails to capture non-linear health crises and extreme mortality conditions.

#### Sensitivity to Parameters

- **Learning rate of 0.002** chosen.
- High learning rate causes divergence and NaN values.
- Very low learning rate increases training time.
- Optimal learning rate balances convergence speed and stability.

#### Where and Why Errors Occur

- Major errors occur for countries with very low life expectancy (< 50 years).
- Model overpredicts because most data lies between 65–80 years, biasing the line of best fit toward the majority.
- Linear regression cannot model compounding health crises.

---

### Part 4: Unsupervised Learning & Reflection

#### Techniques Used

- **K-Means clustering** (K = 3)
- **PCA** (2 components) for visualization

#### Clustering Insights

1. **Low development / high health risk:** Lowest life expectancy (mean ≈ 59).
2. **Moderately developed:** Reduced mortality, improved life expectancy (mean ≈ 72).
3. **Highly developed nations:** Highest life expectancy (mean ≈ 79).

#### PCA Interpretation

- **PC1: Socio-economic development** (Schooling, Income composition, BMI, Status).
- **PC2: Public health vulnerability** (Infant deaths, Under-five deaths, Measles).

#### Structure Revealed

- PCA shows clear separation driven primarily by **PC1**.
- Smooth transition across clusters indicates gradual development rather than abrupt change.

#### How Unsupervised Learning Helps Supervised Modeling

- PCA identifies most influential features, allowing feature reduction.
- K-Means cluster labels can be added as new features.
- Enables different prediction baselines for different country groups.
- Reduces overprediction for low-life-expectancy outliers.
