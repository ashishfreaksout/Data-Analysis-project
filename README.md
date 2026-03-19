# Olympics Medals vs GDP

A small statistics project exploring whether a country's economic strength is associated with Olympic medal outcomes, using GDP, population, athlete counts, and medal totals.

## Project overview

This project analyzes Olympic medal results against national economic indicators to examine whether countries with stronger economies tend to win more medals. The analysis includes exploratory visualizations, simple linear regression, regression diagnostics, and a log transformation attempt when the original model assumptions were not well satisfied.

## Research question

Is there a meaningful relationship between a country's GDP and its Olympic medal performance?

## Dataset and feature engineering

The project uses an Olympics dataset with variables including:

- country
- country_code
- region
- gold
- silver
- bronze
- total
- gdp
- gdp_year
- population
- athletes

Two additional variables were created in the project:

- `total_gdp = gdp * population`
- `medal_pct = total / athletes`

The project also converted `country`, `country_code`, and `region` into factors before analysis.

## Tools used

- **R**
- **ggplot2**
- **dplyr**
- Base R modeling with `lm()`
- R Markdown / knitted report workflow

## What was done

### 1. Exploratory analysis

The project first summarized the data and created visualizations such as:

- GDP per capita vs region
- Total medals vs region
- Total medals vs total GDP
- Gold medals vs total GDP
- Silver medals vs total GDP
- Bronze medals vs total GDP

These plots were used to visually inspect whether wealthier countries tended to earn more medals and whether regional differences existed.

### 2. Initial linear model

A simple linear regression was fit using:

```r
lm(total ~ total_gdp, data = olympics)
```

### 3. Assumption checks

The report checked:

- linearity
- normality of residuals
- constant variance
- independence

The diagnostics suggested that several assumptions were not well met in the original model.

### 4. Transformation attempt

Because the original GDP distribution was highly skewed, the project created:

```r
log_total_gdp = log(total_gdp)
```

and refit the model as:

```r
lm(total ~ log_total_gdp, data = olympics)
```

The goal was to see whether transforming GDP would improve the regression assumptions.

## Key results

### Original model: `total ~ total_gdp`

Main results reported:

- coefficient for `total_gdp`: positive and statistically significant
- p-value: `< 2e-16`
- R-squared: `0.7393`
- adjusted R-squared: `0.7364`

Interpretation:

- Countries with higher total GDP tended to have higher total medal counts.
- The relationship was statistically significant in the fitted model.
- The model explained a substantial portion of the variation in medal totals.

### Transformed model: `total ~ log_total_gdp`

Main results reported:

- coefficient for `log_total_gdp`: positive and statistically significant
- p-value: `1.61e-10`
- R-squared: `0.3733`
- adjusted R-squared: `0.3661`

Interpretation:

- The relationship remained positive and statistically significant after transformation.
- However, the transformed model explained less variation than the original model.
- The transformation helped address skewness in GDP, but it did not fully solve the modeling issues.

## Main takeaway

The project found evidence of a positive relationship between economic strength and Olympic medal totals: countries with larger total GDP generally won more medals.

At the same time, the report also shows that this relationship should be interpreted carefully because the regression assumptions were not fully satisfied, even after a log transformation.

## Failures, issues, and limitations

This section is intentionally included to document what did **not** go perfectly.

### 1. Regression assumptions were not fully met

The report explicitly notes that for `total_gdp`, most assumptions were not met, which is why a log transformation was attempted.

### 2. Log transformation did not completely fix the problem

Although `log_total_gdp` reduced skewness, the residual diagnostics still showed issues. The transformed model also had a much lower R-squared than the original model, which means it did not provide a clearly better overall fit.

### 3. Simple linear regression may be too limited

Using GDP alone likely leaves out important factors that affect medal counts, such as:

- number of athletes sent
- sports investment and infrastructure
- host-country effects
- training systems
- population size
- regional or policy differences

A multiple regression model may have been more realistic.

### 4. Medal totals are count data

Total medals are count outcomes, so a plain linear model may not be the best modeling choice. A count-based model such as Poisson or negative binomial regression could be worth exploring in future work.

### 5. Heavy influence of extreme countries

The scatterplots suggest that a few very large economies with high medal totals may strongly influence the fitted relationship. That means the trend may be partly driven by outliers or high-leverage observations.

### 6. Some report/output issues

A few small report-quality issues appeared in the project:

- `toal_gdp` appears once as a typo before `total_gdp`
- chart files were repeatedly saved with names like `chart1.png`, which could overwrite earlier plots
- one plot label says "GDP per Capita vs Region" while the axis label says "Total GDP", which could confuse readers

These do not invalidate the analysis, but they are useful cleanup points before publishing a polished version.

## What I learned

This project was useful for practicing:

- feature engineering
- exploratory visualization
- simple linear regression
- diagnostic checks
- transforming skewed predictors
- interpreting statistical significance carefully

It also reinforced an important lesson:

> A statistically significant relationship does not automatically mean the model is a perfect one.

## Future improvements

Good next steps for this project would be:

- use multiple regression with GDP, population, and athletes together
- test interactions or regional effects
- evaluate outliers and leverage more formally
- try count-data models for medals
- improve plot labeling and file naming
- write a clearer final conclusion separating statistical significance from model quality

## Project structure

Suggested GitHub structure:

```text
.
├── README.md
├── data/
├── scripts/
├── output/
├── figures/
└── report/
```

## Reproducibility notes

To reproduce the analysis:

1. Load the Olympics dataset
2. Convert categorical fields to factors
3. Create `total_gdp` and `medal_pct`
4. Run exploratory plots
5. Fit `lm(total ~ total_gdp)`
6. Check assumptions
7. Create `log_total_gdp`
8. Fit `lm(total ~ log_total_gdp)`
9. Compare diagnostics and interpret the results

## Author

Ashish Ashish  
Statistics / Data Science project
