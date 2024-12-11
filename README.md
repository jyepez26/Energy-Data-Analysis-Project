<h1>Energy Data Analyis Project</h1>
Project for DSC 80 at UCSD

By Jonathan Yepez

## Introduction
Throughout this project, I explored a dataset on major power outages in different states throughout the U.S. containing data from January 2000-July 2016. The dataset, containing fifty-five total variables, was accessed from Purdue University’s Laboratory on Advances in Sustainable Infrastructure, at the url https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks. This dataset can be used to recognize and explore trends and patterns of the major outages in the U.S. as well as to identify and assess the risk predictors associated with varying power outages. The data also provide characteristics of the states in the U.S., including their climate and topographical characteristics, electricity consumption patterns, population, economic characteristics, and land-cover characteristics. 

I will begin my research by cleaning my dataset and performing exploratory data analysis to gain a better understanding of the data I am working with and some basic patterns. After this, I will move on to assess the missingness of values in the data and then conduct a hypothesis test to explore equity among distributions.

Next, I will analyze my research question. Personally, I am heavily interested in economics and how important it is to our country. Therefore, I wanted to learn more about how the economic factors play a part in the power outages in this country and how that knowledge can be used to help develop better infrastructure and economic structures to reduce or better predict outages.

Therefore, my research question is: how do economic and geographic factors impact the causes and properties of power outages in the United States? First, I will conduct a hypothesis test to explore economic equity among distributions. Lastly, I will create a model that predicts the cause of a power outage based on economic, geographic, and demographic information.

The original DataFrame contains 1534 rows, each corresponding to a unique outage, and 57 columns containing features. Throughout my analysis, I will only focus on a subset of the columns that will be important to my research.

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
