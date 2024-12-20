<h1>Energy Data Analyis Project</h1>
Project for DSC 80 at UCSD

By Jonathan Yepez

## Introduction
Throughout this project, I explored a dataset on major power outages in different states throughout the U.S. containing data from January 2000-July 2016. The dataset, containing fifty-five total variables, was accessed from Purdue University’s Laboratory on Advances in Sustainable Infrastructure, at the url https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks. This dataset can be used to recognize and explore trends and patterns of the major outages in the U.S. as well as to identify and assess the risk predictors associated with varying power outages. The data also provide characteristics of the states in the U.S., including their climate and topographical characteristics, electricity consumption patterns, population, economic characteristics, and land-cover characteristics. 

I will begin my research by cleaning my dataset and performing exploratory data analysis to gain a better understanding of the data I am working with and some basic patterns. After this, I will move on to assess the missingness of values in the data and then conduct a hypothesis test to explore equity among distributions.

Next, I will analyze my research question. Personally, I am heavily interested in economics and how important it is to our country. Therefore, I wanted to learn more about how the economic factors play a part in the power outages in this country and how that knowledge can be used to help develop better infrastructure and economic structures to reduce or better predict outages.

Therefore, my research question is: how do economic and geographic factors impact the causes and properties of power outages in the United States? First, I will conduct a hypothesis test to explore economic equity among distributions. Lastly, I will create a model that predicts the cause of a power outage based on economic, geographic, and demographic information.

The original DataFrame contains 1534 rows, each corresponding to a unique outage, and 57 columns containing features. Throughout my analysis, I will only focus on a subset of the columns that will be important to my research.

| Column                    | Description                                                              |
|---------------------------|--------------------------------------------------------------------------|
| `'YEAR'`                  | Year of the outage                                                      |
| `'MONTH'`                 | Month of the outage                                                     |
| `'POSTAL.CODE'`           | State where the outage occurred                                         |
| `'CLIMATE.REGION'`        | U.S. Climate region (defined by the National Centers for Environmental Information) |
| `'CLIMATE.CATEGORY'`      | Climate episodes corresponding to the years. Based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI) |
| `'ANOMALY.LEVEL'`         | Oceanic El Niño/La Niña index for seasonal cold and warm episodes       |
| `'OUTAGE.START.DATE'`     | Day of the year when the outage began                                   |
| `'OUTAGE.START.TIME'`     | Time of day when the outage began                                       |
| `'OUTAGE.RESTORATION.DATE'` | Day of the year when power was fully restored                         |
| `'OUTAGE.RESTORATION.TIME'` | Time of day when power was fully restored                             |
| `'CAUSE.CATEGORY'`        | Event category causing the outage                                       |
| `'OUTAGE.DURATION'`       | Duration of the outage (in minutes)                                     |
| `'DEMAND.LOSS.MW'`        | Peak demand lost during the outage (in MW)                              |
| `'CUSTOMERS.AFFECTED'`    | Number of customers impacted by the outage                              |
| `'PC.REALGSP.STATE'`      | Per capita real gross state product (GSP) in the U.S. state             |
| `'TOTAL.REALGSP'`         | Real GSP (measurement of state's output) contributed by all industries  |
| `'TOTAL.PRICE'`           | Average electricity price in the state (cents/kWh)                      |
| `'TOTAL.SALES'`           | Total electricity consumption in the state (MWh)                        |
| `'RES.SALES'`             | Electricity consumption in the residential sector                       |
| `'POPPCT_URBAN'`          | Percentage of the state's population living in urban areas              |

---

## Data Cleaning and Exploratory Data Analysis
### **Data Cleaning**
The first step in my research is to clean the data, ensuring that it is formatted appropriately for proper exploration and analysis.

Step 1:

After reading the excel file into a pandas dataframe, the format was messy–containing empty rows and meaningless column names. Therefore, I removed the empty rows and changed the column headers to their correct names. There was an additional row that included the units of each columns’ values, which I removed but stored in a dictionary to potentially use later. I then dropped all the columns that were not relevant, accounting for redundant information, only keeping the ones that I will be using in my research: 
`TOTAL.PRICE`, `CAUSE.CATEGORY`, `PC.REALGSP.STATE`, `POSTAL.CODE`, `TOTAL.SALES`, `OUTAGE.DURATION`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `TOTAL.REALGSP`, `CUSTOMERS.AFFECTED`, `DEMAND.LOSS.MW`, `POPPCT_URBAN`, `RES.SALES`, `MONTH`, `YEAR`, `OUTAGE.START.DATE`,`OUTAGE.START.TIME`, `OUTAGE.RESTORATION.TIME`, `OUTAGE.RESTORATION.DATE`, `ANOMALY.LEVEL`.

Step 2:

My next step was to create a single column for `OUTAGE.START` and `OUTAGE.RESTORATION`. To do this, I combined the values `OUTAGE.START.DATE` and the `OUTAGE.START.TIME` columns into Timestamp objects and saved them to a new column called `OUTAGE.START`. I repeat this same process to combine `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into one column of Timestamp objects called `OUTAGE.RESTORATION`. I then dropped the original two columns, as their values are now stored in the combined columns. This step allows for easier access and utilization of the start and restoration values for outages. 

Step 3:

My last step was to check the extent of outages columns, `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW`, for values of 0. It is very unlikely that any of these values will be reported zero, as a power outage will have to last some duration of time, which in turn leads to peak demand being lost and customers being impacted. Therefore, a value of 0 in these features would be suggestive of a missing value, and I will replace these values with np.nan instead.

The first few rows and columns of the cleaned dataframe, titled "outages", are showed below:

| **YEAR** | **MONTH** | **POSTAL.CODE** | **OUTAGE.START**      | **OUTAGE.RESTORATION** | **OUTAGE.DURATION** | **CUSTOMERS.AFFECTED** |
|----------|-----------|-----------------|-----------------------|------------------------|---------------------|-----------------------|
| 2011     | 7         | MN              | 2011-07-01 17:00:00   | 2011-07-03 20:00:00    | 3060                | 70000                 |
| 2014     | 5         | MN              | 2014-05-11 18:38:00   | 2014-05-11 18:39:00    | 1                   | NaN                   |
| 2010     | 10        | MN              | 2010-10-26 20:00:00   | 2010-10-28 22:00:00    | 3000                | 70000                 |
| 2012     | 6         | MN              | 2012-06-19 04:30:00   | 2012-06-20 23:00:00    | 2550                | 68200                 |
| 2015     | 7         | MN              | 2015-07-18 02:00:00   | 2015-07-19 07:00:00    | 1740                | 250000                |



### **Exploratory Data Analysis**

### Univariate Analysis
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
<iframe
  src="assets/geospatial_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Analysis
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


### Aggregation
For the aggregation, I wanted to begin to explore some geographical features. Therefore, I created a pivot table which aggregated the mean Outage Duration based on the Climate Region and the Cause Category. More simply, this pivot table shows the average duration of outages in each climate region for each cause category.

| `CLIMATE.REGION`    | **equipment failure** | **fuel supply emergency** | **intentional attack** | **islanding** | **public appeal** | **severe weather** | **system operability disruption** |
|:----------------------:|:---------------------:|:-------------------------:|:----------------------:|:-------------:|:-----------------:|:------------------:|:--------------------------------:|
| **Central**            | 322                   | 10,035.2                  | 490.25                 | 125.333       | 1,410             | 3,299.63           | 2,695.2                          |
| **East North Central** | 26,435.3              | 33,971.2                  | 2,501.11               | 1             | 733               | 4,434.82           | 2,610                            |
| **Northeast**          | 269.75                | 14,629.6                  | 264.68                 | 881           | 2,655             | 4,429.9            | 773.5                            |
| **Northwest**          | 702                   | 1                         | 488.831                | 73.3333       | 898               | 4,838              | 141                              |
| **South**              | 295.778               | 17,482.5                  | 337.667                | 493.5         | 1,163.98          | 4,391.35           | 899.385                          |
| **Southeast**          | 554.5                 | NaN                       | 504.667                | NaN           | 2,865.4           | 2,685.71           | 180.6                            |
| **Southwest**          | 113.8                 | 76                        | 274.678                | 2             | 2,275             | 11,572.9           | 370.375                          |
| **West**               | 524.81                | 6,154.6                   | 886.267                | 214.857       | 2,028.11          | 2,928.37           | 363.667                          |
| **West North Central** | 61                    | NaN                       | 47                     | 68.2          | 439.5             | 2,442.5            | NaN                              |


---


| `**CLIMATE.CATEGORY**`    | **equipment failure** | **fuel supply emergency** | **intentional attack** | **islanding** | **public appeal** | **severe weather** | **system operability disruption** |
|    `**CLIMATE.REGION**`    |                       |                           |                        |               |                   |                    |                                  |
|:--------------------:|:---------------------:|:-------------------------:|:----------------------:|:-------------:|:-----------------:|:------------------:|:--------------------------------:|
| **Central**          | 322                   | 10,035.2                  | 490.25                 | 125.333       | 1,410             | 3,299.63           | 2,695.2                          |
| **East North Central** | 26,435.3            | 33,971.2                  | 2,501.11               | 1             | 733               | 4,434.82           | 2,610                            |
| **Northeast**        | 269.75                | 14,629.6                  | 264.68                 | 881           | 2,655             | 4,429.9            | 773.5                            |
| **Northwest**        | 702                   | 1                         | 488.831                | 73.3333       | 898               | 4,838              | 141                              |
| **South**            | 295.778               | 17,482.5                  | 337.667                | 493.5         | 1,163.98          | 4,391.35           | 899.385                          |
| **Southeast**        | 554.5                 | NaN                       | 504.667                | NaN           | 2,865.4           | 2,685.71           | 180.6                            |
| **Southwest**        | 113.8                 | 76                        | 274.678                | 2             | 2,275             | 11,572.9           | 370.375                          |
| **West**             | 524.81                | 6,154.6                   | 886.267                | 214.857       | 2,028.11          | 2,928.37           | 363.667                          |
| **West North Central** | 61                  | NaN                       | 47                     | 68.2          | 439.5             | 2,442.5            | NaN                              |


---

## Assesment of Missingness
### NMAR Analysis
Although there are multiple columns in the data that contain missing values, one of the columns that is likely Not Missing at Random (NMAR) is `CLIMATE.REGION`. This data is likely NMAR because the missing values are most likely an effect of the data collection methods, which involved pulling data from the National Centers for Environmental Information. If the NCEI did not have data on the region for a certain outage, then those missing values would be reflected in this dataset. 

In order to see if the data is actually Missing at Random (MAR), I would collect the date in which the data of the climate region for the outage was published, and then conduct analysis to see if the missingness of the climate region is MAR dependent on when the data was published.

### Missingess Dependency
To test missingness dependency, I am going to focus on the peak demand loss during an outage, `DEMAND.LOSS.MW`. I will be testing these values against the `CLIMATE.CATEGORY` and `CLIMATE.REGION` columns.

MAR Analysis of `DEMAND.LOSS.MW` VS. `CLIMATE.CATEGORY`:

First, I chose to look at the distribution of CLIMATE.CATEGORY when demand loss is missing and not missing.
Null Hypothesis: The distribution of the climate category column is the same when peak demand loss is missing and when it is not missing.
Alternate Hypothesis: The distribution of the climate category column is different when peak demand loss is missing and when it is not missing.

<iframe
  src="assets/demand_vs_climate_category_proportions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I found an observed TVD of 0.03, which has a p value of 0.55. The empirical distribution of the TVDs is shown in the plot below. At this p value, I fail to reject the null hypothesis, as it is not close to a statistically significant p value of 0.05. Based on the empirical distribution, you can clearly see that our observed TVD is a normal value to be observed, and therefore we cannot reject the null hypothesis that the distribution of the climate category column is the same when peak demand loss is missing and when it is not missing.

<iframe
  src="assets/demand_vs_climate_category_emp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



MAR Analysis of `DEMAND.LOSS.MW` VS. `CLIMATE.REGION`:

Next, I decided to look at the distribution of CLIMATE.REGION when demand loss is missing and not missing.
Null Hypothesis: The distribution of the climate region column is the same when peak demand loss is missing and when it is not missing.
Alternate Hypothesis: The distribution of the climate region column is different when peak demand loss is missing and when it is not missing.

Below, I plotted the Proportion Plot Simulated Under the Null Hypothesis and the Observed Proportion Plot. If you pay attention to the West Region, there is a significant difference between the two distributions.

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

I found an observed TVD of 0.23, which has a p value of 0.0. The empirical distribution of the TVDs is shown in the plot below. At this p value, I am able to reject the null hypothesis, as 0.0 is a statistically significant p value. Therefore, we reject that the distribution of the climate region column is the same when peak demand loss is missing and when it is not missing.

<iframe
  src="assets/region_vs_climate_emp_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Hypothesis Testing
In this hypothesis test, I will be testing whether the outage duration is greater on average in a state where the GSP is below the median. The relevant columns for this hypothesis test are `OUTAGE.DURATION` and `PC.REALGSP.STATE`.

**NULL HYPOTHESIS**: On average, the duration of an outage in a state where the Per Capita GSP (Gross State Product) is larger than or equal to $48370 (the median of all GSPs), is the same as the duration of an outage in a state where the Per Capita GSP is less than $48370.

**ALTERNATE HYPOTHESIS**: On average, the duration of an outage in a state where the Per Capita GSP (Gross State Product) is larger than $48370 (the median of all GSPs), is shorter as the duration of an outage in a state where the Per Capita GSP is less than $48370.

**TEST STATISTIC**: Difference in Means (Mean Outage Duration where GSP >= 48370 - Mean Outage Duration where GSP < 48370

I performed a permutation test with 500 simulations (increasing past this threshold did not change the results), to create an empirical distribution of my test statistic generated under the null hypothesis.

I achieved a p value of 0.0, which is statistically significant, and therefore I rejected the null hypothesis. From these results we can conclude that most likely on average, the duration of an outage where the Per Capita Gross State Product is in the lower 50%, will be longer than its counterpart. 

From an economic standpoint, I find this discovery very interesting, as it demonstrates that lower economic power in the area where the outage occurs can actually have a negative impact on how destructive the power outage is. Earlier on, we failed to see this pattern, so it is exciting that we can see the pattern actually does exist when you dig deeper into the data!

Below, you can see the plot of the empirical distribution of the test statistic from the hypothesis test I conducted.

<iframe
  src="assets/updated_hypo_test_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Framing a Prediction Problem
My model will be developed to predict the cause of a power outage. This will be a decision tree classification because we are trying to predict any of the different outages cause categories.

The metric I am using to evaluate my model is the accuracy, because it gives a straightforward and easy to interpret score of my model. My model will consist of various features, and while there is some imbalance within the classes we are predicting, it is sometimes better to overcomplicate the scoring metric. When I used precision, recall, or the F1 score, I did not get too large of a difference in score, and therefore decided it was better to just keep it simpler.

At the time of prediction, we will know the `CLIMATE.REGION`,`RES.SALES`,`OUTAGE.DURATION`,`CUSTOMERS.AFFECTED`, `MONTH`,`POPPCT_URBAN`,`TOTAL.PRICE`. This information will allow us to predict what the cause of a major power outage is.

---

## Baseline Model
My model is a decision tree classifier using the features climate region, res sales, outage duration, poppct_urban to predict the cause of a major outage. This information would provide economists and policymakers with information on how certain geographical and economic factors impact the outages in their area.

The features are: `CLIMATE.REGION` (nominal), `RES.SALES` (quantitative), `OUTAGE.DURATION` (quantitative), and `POPPCT_URBAN` (quantitative). I chose these specific features because `CLIMATE.REGION` indicates the geographic area that the outage occurred in, `RES.SALES` provides economic information about the electricity customers purchasing power, `OUTAGE.DURATION `to account for the severity of the outage, and POPPCT_URBAN since the areas where there is a higher urban population actually can lead to more negative impacts on the energy and power and therefore can lead to more major power outages caused by severe weather and other common causes. Also, I used `RES.SALES` since when demand for electricity exceeds the available supply, a controlled outage may be necessary.

I used One Hot Encoding to convert the Climate Region to a quantitative column.

The performance of my baseline model was not the best, but definitely passable, with an accuracy of 0.64.

---

## Final Model
My final model used these features: `CLIMATE.REGION`,`RES.SALES`,`OUTAGE.DURATION`,`CUSTOMERS.AFFECTED`, `MONTH`,`POPPCT_URBAN`,`TOTAL.PRICE`.

I added `CUSTOMERS.AFFECTED` (quantitative) as another measure of the severity of the outage. I included `MONTH` (ordinal) because many intentional attacks occur due to hot, dry days with sustained wind, and also severe weather outages are more likely during the winter due to snow, rain, and blizzards. `TOTAL.PRICE` (quantitative) is another specific economic feature that impacts the energy consumption, and therefore the type of outage that could occur.

For the feature engineering, I used a custom binarizer to convert the month to either spring/summer or fall/winter, and to standardize the outage duration and customers affected.

I used GridSearchCV, with the default cross validation of 5-fold, in order to find the best hyperparameters for the DecisionTreeClassifier. The best hyperparameters ended up being:

criterion: entropy
max_depth: 10
min_samples_split: 20

I used the accuracy score to measure the performance of my model. I got an accuracy of 0.814. Since the accuracy increased significantly from the baseline model to the final model, this demonstrates better performance of the final model.

---

## Fairness Analysis
My groups for the fairness analysis are longer vs shorter outages. This is defined as an anomaly that is greater than 0.0, vs that are less than 0.0.

I decided on these groups because I was curious if the cause category (which is predicted by my model) would evaluate these differently.

My evaluation metric will be accuracy score since it has been a good evaluation of my model. I will use permutation tests to calculate the accuracy score for higher vs lower anomaly levels (that are randomly shuffled) and then compare this absolute difference to my initial observed absolute difference.

Null Hypothesis: The model is fair. Its accuracy scores for higher and lower anomalies are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: The model is unfair. Its accuracy score for higher anomalies is significantly different from the accuracy for lower ones.

I performed a permutation test with 500 trials. My significance level is the standard 0.05, and I got a p_value of around 0.5 so I fail to reject the null hypothesis. Therefore, my model is fair between higher and lower anomaly levels.

The figure below shows the distribution of the statistic.
<iframe
  src="assets/fairness_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
