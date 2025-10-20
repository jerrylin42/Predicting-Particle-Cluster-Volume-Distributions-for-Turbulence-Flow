# Predicting-Particle-Cluster-Volume-Distributions-for-Turbulence-Flow

Understanding mean, standard deviation, skewness, and kurtosis of particle cluster volume distributions using observed data.

# Project Outline

The dataset consists of n = 89 simulations, each conducted at a different set of input parameters. There are three parameters (predictors): Reynolds number Re, gravitational acceleration Fr, and particle characteristic St. The raw response variable (obtained from numerical simulation and cluster analysis) is a probability distribution for particle cluster volumes. Since probability distributions are hard to work with, collaborators instead summarized these distributions by their first four raw moments.

We want to explore how fluid turbulence (quantified by Reynolds number or Re), gravitational acceleration (quantified by Froud number or Fr) and particles’ characteristics (e.g. size, density which is quantified by Stokes number or St) affect the spatial distribution and clustering of particles in an idealized turbulence (homogeneous isotropic turbulence or HIT).

## **Exploratory Data Analysis Summary**

### **Data Cleaning & Variable Transformations**

-   **Froude Number (Fr)**

    -   **Issue:** Some simulations had Fr = ∞ (no gravity), which cannot be used directly in regression models.

    -   **Transformation:**

        $$
        Fr_{\text{invlogit}} = \frac{1}{1 + e^{-Fr}}
        $$

    -   **Purpose:** This maps values of $Fr$ into the interval $(0, 1)$ and keeps Fr numeric and continuous while handling ∞ values.

-   **Reynolds Number (Re)**

    -   **Issue:** Re had only three discrete values, so taking it as continuous would incorrectly imply a linear relationship.

    -   **Transformation:** Converted Re into a categorical variable.

    -   **Purpose:** Prevents the model from assuming a false linear relationship and treats turbulence regimes as distinct categories.\

-   **Right-Skewed Variables**

    -   **Observation:** The response variables (mean, variance, skewness, kurtosis) and some predictors (St, Fr_invlogit) were strongly right-skewed.

    -   **Recommended Transformation:** Apply log transformations to reduce skew and stabilize variance:

        -   log(mean)

        -   log(variance)

        -   log(skewness)

        -   log(kurtosis)

        -   log(St)

        -   log(Fr_invlogit)

### **Findings**

-   **Pairwise Relationships (ggpairs):**

    -   Collinearity between the three predictors (St, Re, Fr_invlogit) is not a major concern.

    -   Some predictors show nonlinear relationships with responses, suggesting the need for transformations or nonlinear models.

-   **Histograms:**

    -   Confirmed strong right skew in several response variables, further supporting the use of log transformations.

### **Baseline Regression Results**

-   Built four separate linear models:

    -   mean ~ St + Re + Fr_invlogit

    -   variance ~ St + Re + Fr_invlogit

    -   skewness ~ St + Re + Fr_invlogit

    -   kurtosis ~ St + Re + Fr_invlogit

-   **Findings:**

    -   The model for mean performed reasonably well using basic linear regression.

    -   Models for variance, skewness, and kurtosis had weaker ( R\^2 ) values, indicating the need for log-transformations, interaction terms, or regularization.

### **Planned Modeling Workflow**

1.  Start with a full model using log-transformed variables and interaction terms.

2.  Use best-subset selection (AIC or cross-validation) to identify the strongest predictors

3.  Compare OLS and ridge regression to assess performance improvements.

4.  Potentially explore lasso regression if further feature selection is needed.

**Summary:**

-   Converted Re to a categorical variable.

-   Transformed Fr using an inverse logit to handle ∞ values.

-   Recommended log transformations for skewed predictors and responses.

-   Verified that predictor collinearity is low.

-   Established baseline regression models and planned a refinement workflow.
