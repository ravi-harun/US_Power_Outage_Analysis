# A Data Analysis of Major U.S. Power Outages (2000-2016)
**By:**
- Michael Tang (mjtang@ucsd.edu)
- Ravi Harun (rharun@ucsd.edu)

## Introduction

Power outages disrupt the daily lives of millions and result in billions of dollars in economic losses annually, particularly in regions prone to extreme weather events [1]. This analysis investigates: *What factors have the most influence on the duration of a power outage?* Using historical outage data from January 2000 to July 2016, we seek to understand trends and potential patterns.

First, we imported the power outages dataset downloaded from [Purdue University](https://engineering.purdue.edu/LASCI/research-data/outages). There are 1534 rows in the raw dataset.

The table below contains a data dictionary on relevant columns for our analysis (using our cleaned data).

|Variable name|Description|
|---|---|
|`'year'`|Indicates the year when the outage event occurred|
|`'month'`|Indicates the month when the outage event occurred| 
|`'us_state'`|Represents all the states in the continental U.S.|
|`'postal_code'`|Represents the postal code of the U.S. states|
|`'nerc_region'`|The North American Electric Reliability Corporation (NERC) regions involved in the outage event|
|`'climate_region'`|U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)|
|`'anomaly_level'`|This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) [6]|
|`'climate_category'`|This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)|
|`'cause_category'`|Categories of all the events causing the major power outages|
|`'cause_category_detail'`|Detailed description of the event categories causing the major power outages|
|`'hurricane_names'`|If the outage is due to a hurricane, then the hurricane name is given by this variable|
|`'outage_duration'`|Duration of outage events (in minutes)|
|`'demand_loss_mw'`|Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]|
|`'customers_affected'`|Number of customers affected by the power outage event|
|`'total_price'`|Average monthly electricity price in the U.S. state (cents/kilowatt-hour)|
|`'population'`|Population in the U.S. state in a year

<iframe
  src="assets/outage_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


| cause_category     | climate_category   |
|:-------------------|:-------------------|
| severe weather     | normal             |
| intentional attack | normal             |
| severe weather     | cold               |
| severe weather     | normal             |
| severe weather     | warm               |


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning

We dropped extra columns that remained from converting the raw Excel dataset into a Pandas DataFrame. Then, we cleaned up the column names to make them easier to utilize when performing data wrangling and EDA. 

The `outage_start_date` and `outage_start_time` columns were combined into a single Timestamp column called `outage_start`. Similarly, the `outage_restoration_date` and `outage_restoration_time` columns were combined into a new column called `outage_restoration`. Subsequently, the old columns were dropped to avoid keeping duplicate information.

|   year |   month | us_state   | postal_code   | nerc_region   | climate_region     |   anomaly_level | climate_category   | cause_category     | cause_category_detail   |   hurricane_names |   outage_duration |   demand_loss_mw |   customers_affected |   res_price |   com_price |   ind_price |   total_price |   res_sales |   com_sales |   ind_sales |   total_sales |   res_percen |   com_percen |   ind_percen |   res_customers |   com_customers |   ind_customers |   total_customers |   res_cust_pct |   com_cust_pct |   ind_cust_pct |   pc_realgsp_state |   pc_realgsp_usa |   pc_realgsp_rel |   pc_realgsp_change |   util_realgsp |   total_realgsp |   util_contri |   pi_util_ofusa |   population |   poppct_urban |   poppct_uc |   popden_urban |   popden_uc |   popden_rural |   areapct_urban |   areapct_uc |   pct_land |   pct_water_tot |   pct_water_inland | outage_start        | outage_restoration   |
|-------:|--------:|:-----------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota  | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 | Minnesota  | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 | Minnesota  | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 | Minnesota  | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 | Minnesota  | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |


### Univariate Analysis

This bar plot illustrates the number of power outages across each climate region, highlighting the `Northeast` region's significantly higher outage count compared to all others.

This bar plot shows the number of power outages distributed by `cause category`. Severe weather and intentional attack outages are the two most common causes. 

### Bivariate Analysis

As we saw previously, severe weather and intentional attacks are the two leading causes of power outages in the dataset. While the median electricity price for outages from both causes is similar, severe weather outages exhibit greater price variability. This suggests that the timing and location of severe weather-related outages may differ more widely.

### Interesting Aggregates

This table presents the mean values of key metrics for power outages, grouped by climate region. It emphasizes regional variations in outage duration, demand loss, and customers affected.

This pivot table summarizes the frequency of power outages by cause category across different climate regions, offering insights into the distribution of outage causes regionally.

Expanding on the distribution of outage causes, this pivot table shows how these causes vary across different climate conditions.


## Assessment of Missingness


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis
