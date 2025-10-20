# Particle Clustering in Turbulent Flows

## Project Overview

This project investigates how fluid turbulence and particle characteristics affect the spatial distribution and clustering of particles in homogeneous isotropic turbulence (HIT). The analysis is based on **n = 89** numerical simulations, each conducted with different input parameters.

### Dataset Structure

**Predictors (Input Parameters):**
- **Reynolds number (Re):** Quantifies fluid turbulence intensity
- **Froude number (Fr):** Quantifies gravitational acceleration effects
- **Stokes number (St):** Quantifies particle characteristics (size, density)

**Response Variables:**
Raw simulation output consists of probability distributions for particle cluster volumes. These distributions are summarized using their first four raw moments:
- Mean
- Variance
- Skewness
- Kurtosis

---

## Exploratory Data Analysis

### Data Cleaning & Variable Transformations

#### Froude Number (Fr)
**Issue:** Some simulations had `Fr = ∞` (no gravity), which cannot be used directly in regression models.

**Transformation:**
```
Fr_invlogit = 1 / (1 + exp(-Fr))
```

**Purpose:** Maps Fr values into the interval (0, 1), maintaining continuity while handling infinite values.

#### Reynolds Number (Re)
**Issue:** Re had only three discrete values. Treating it as continuous would incorrectly assume a linear relationship.

**Transformation:** Converted Re into a categorical variable.

**Purpose:** Treats different turbulence regimes as distinct categories rather than assuming linear progression.

#### Right-Skewed Variables
**Observation:** Response variables (mean, variance, skewness, kurtosis) and some predictors (St, Fr_invlogit) exhibited strong right skew.

**Recommended Transformations:** Apply log transformations to reduce skew and stabilize variance:
- `log(mean)`
- `log(variance)`
- `log(skewness)`
- `log(kurtosis)`
- `log(St)`
- `log(Fr_invlogit)`

### Key Findings

**Pairwise Relationships (ggpairs):**
- Collinearity between predictors (St, Re, Fr_invlogit) is not a major concern
- Some predictors show nonlinear relationships with responses, suggesting the need for transformations or nonlinear models

**Histograms:**
- Confirmed strong right skew in several response variables, supporting log transformations

---

## Baseline Regression Results

Four separate linear models were constructed:
1. `mean ~ St + Re + Fr_invlogit`
2. `variance ~ St + Re + Fr_invlogit`
3. `skewness ~ St + Re + Fr_invlogit`
4. `kurtosis ~ St + Re + Fr_invlogit`

**Results:**
- The model for **mean** performed reasonably well using basic linear regression
- Models for **variance**, **skewness**, and **kurtosis** had weaker R² values, indicating need for:
  - Log transformations
  - Interaction terms
  - Regularization techniques

---

## Planned Modeling Workflow

1. **Full Model Development:** Build models using log-transformed variables and interaction terms
2. **Feature Selection:** Apply best-subset selection using AIC or cross-validation to identify strongest predictors
3. **Model Comparison:** Compare ordinary least squares (OLS) and ridge regression performance
4. **Advanced Methods:** Explore lasso regression if additional feature selection is needed

---

## Summary of Analysis Decisions

✓ Converted Re to a categorical variable  
✓ Transformed Fr using inverse logit to handle ∞ values  
✓ Recommended log transformations for skewed predictors and responses  
✓ Verified low predictor collinearity  
✓ Established baseline regression models  
✓ Defined systematic refinement workflow

---

## Next Steps

- Implement log transformations across all models
- Test interaction effects between predictors
- Perform model selection using information criteria
- Validate final models using cross-validation
- Compare regularized vs. standard regression approaches
