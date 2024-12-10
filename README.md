<h1>Energy Data Analyis Project</h1>
Project for DSC 80 at UCSD

By Jonathan Yepez

## Introduction
```
Here is some cool code!
```

## Univariate Analysis
Here is the plot for Average Monthly Electriciy Price in the US:
<iframe
  src="assets/avg_electricity_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Here is the plot for Power Outage Cause Categories Distribution:
<iframe
  src="assets/outage_cause_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Bivariate Analysis
Here is the plot of Customers Affected vs Total GSP:
<iframe
  src="assets/customers_per_gsp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Here is the plot of Real Per Capita GSP for each State:
<iframe
  src="assets/gsp_per_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Assesment of Missingness
### NMAR Analysis
Although there are multiple columns in the data that contain missing values, one of the columns that is likely Not Missing at Random (NMAR) is `CLIMATE.REGION`. This data is likely NMAR because the missing values are most likely an effect of the data collection methods, which involved pulling data from the National Centers for Environmental Information. If the NCEI did not have data on the region for a certain outage, then those missing values would be reflected in this dataset. 

In order to see if the data is actually Missing at Random (MAR), I would collect the date in which the data of the climate region for the outage was published, and then conduct analysis to see if the missingness of the climate region is MAR dependent on when the data was published.

### MAR Analysis
MAR ANALYSIS OF DEMAND LOSS VS. CLIMATE CATEGORY (Not MAR Dependent)
Proportion Plot:
<iframe
  src="assets/demand_vs_climate_category_proportions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Empirical Distribution Plot:
<iframe
  src="assets/demand_vs_climate_category_emp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

MAR ANALYSIS OF DEMAND LOSS VS. CLIMATE REGION (MAR Dependent)
Proportion Plot Simulated Under the Null Hypothesis:
<iframe
  src="assets/region_vs_demand_random_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Observed Proportion Plot:
<iframe
  src="assets/region_vs_demand_mar_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Empirical Distribution Plot:
<iframe
  src="assets/region_vs_climate_emp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
Here is my empirical distribution:

<iframe
  src="assets/updated_hypo_test_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
