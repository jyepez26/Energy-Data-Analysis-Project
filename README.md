# Energy Data Analysis Project
Project for DSC 80 at UCSD

By Jonathan Yepez

# Introduction


# Assesment of Missingness
## NMAR Analysis
Although there are multiple columns in the data that contain missing values, one of the columns that is likely Not Missing at Random (NMAR) is `CLIMATE.REGION`. This data is likely NMAR because the missing values are most likely an effect of the data collection methods, which involved pulling data from the National Centers for Environmental Information. If the NCEI did not have data on the region for a certain outage, then those missing values would be reflected in this dataset. 

In order to see if the data is actually Missing at Random (MAR), I would collect the date in which the data of the climate region for the outage was published, and then conduct analysis to see if the missingness of the climate region is MAR dependent on when the data was published.

