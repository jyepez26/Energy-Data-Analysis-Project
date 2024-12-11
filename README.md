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
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The first step in my research is to clean the data, ensuring that it is formatted appropriately for proper exploration and analysis.
Step 1:
After reading the excel file into a pandas dataframe, the format was messy–containing empty rows and meaningless column names. Therefore, I removed the empty rows and changed the column headers to their correct names. There was an additional row that included the units of each columns’ values, which I removed but stored in a dictionary to potentially use later. I then dropped all the columns that were not relevant, accounting for redundant information, only keeping the ones that I will be using in my research: 
`TOTAL.PRICE`, `CAUSE.CATEGORY`, `PC.REALGSP.STATE`, `POSTAL.CODE`, `TOTAL.SALES`, `OUTAGE.DURATION`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `TOTAL.REALGSP`, `CUSTOMERS.AFFECTED`, `DEMAND.LOSS.MW`, `POPPCT_URBAN`, `RES.SALES`, `MONTH`, `YEAR`, `OUTAGE.START.DATE`,`OUTAGE.START.TIME`, `OUTAGE.RESTORATION.TIME`, `OUTAGE.RESTORATION.DATE`, `ANOMALY.LEVEL`.

Step 2:
My next step was to create a single column for `OUTAGE.START` and `OUTAGE.RESTORATION`. To do this, I combined the values `OUTAGE.START.DATE` and the `OUTAGE.START.TIME` columns into Timestamp objects and saved them to a new column called `OUTAGE.START`. I repeat this same process to combine `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into one column of Timestamp objects called `OUTAGE.RESTORATION`. I then dropped the original two columns, as their values are now stored in the combined columns. This step allows for easier access and utilization of the start and restoration values for outages. 

Step 3:
My last step was to check the extent of outages columns, `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW`, for values of 0. It is very unlikely that any of these values will be reported zero, as a power outage will have to last some duration of time, which in turn leads to peak demand being lost and customers being impacted. Therefore, a value of 0 in these features would be suggestive of a missing value, and I will replace these values with np.nan instead.

### Exploratory Data Analysis
#### UNIVARIATE ANALYSIS
To begin my exploratory data analysis, I performed a univariate analysis to understand the distributions of certain single properties.

I first wanted to see how many outages there were of each cause category, to see which were the most and least common causes.
<iframe
  src="assets/outage_cause_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then, I wanted to see the average total electricity price across all outages to get a sense of the most common costs as well as the range.
<iframe
  src="assets/avg_electricity_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I then wanted to see how the average total electricity price varied among the different states. *The black states have too few values to have a proper density.

---

#### Bivariate Analysis
For the next step in my exploratory data analysis, I conducted a bivariate analysis in order to examine how certain variables impact each other and take note of any patterns.

To continue to explore various economic characteristics, I wanted to see how the per capita real gross state product–a state’s total output measured per person–varied among different states. There was a notable difference between Washington D.C. and every other state, which stems from the miniscule population size, low variance, and the government’s actions counting as contributions to their GSP.
<iframe
  src="assets/customers_per_gsp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, I was interested to see how the number of customers affected by an outage was impacted by the total real gross state product of the state in which the outage occurred. There was no apparent pattern here, which surprised me a bit; initially I thought there would be a higher amount of customers affected for the lower GSP values, due to the state not having as many resources to reduce the impacts of an outage. *I separated the points by the climate region where the outage occurred, just to make the points more visually understandable. This extra step did seem to reveal that the West climate region tends to have a high total GSP value, which is an interesting note. 
<iframe
  src="assets/gsp_per_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

#### Aggregation
For the aggregation, I wanted to begin to explore some geographical features. Therefore, I created a pivot table which aggregated the mean Outage Duration based on the Climate Region and the Cause Category. More simply, this pivot table shows the average duration of outages in each climate region for each cause category.


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
