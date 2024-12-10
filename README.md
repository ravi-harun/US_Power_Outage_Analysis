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

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning

We dropped extra columns that remained from converting the raw Excel dataset into a Pandas DataFrame. Then, we cleaned up the column names to make them easier to utilize when performing data wrangling and EDA. 

A portion of selected columns from the cleaned DataFrame is displayed below.

|   year |   month | us_state   | climate_region     | cause_category     |   outage_duration |   customers_affected |
|-------:|--------:|:-----------|:-------------------|:-------------------|------------------:|---------------------:|
|   2011 |       7 | Minnesota  | East North Central | severe weather     |              3060 |                70000 |
|   2014 |       5 | Minnesota  | East North Central | intentional attack |                 1 |                  nan |
|   2010 |      10 | Minnesota  | East North Central | severe weather     |              3000 |                70000 |
|   2012 |       6 | Minnesota  | East North Central | severe weather     |              2550 |                68200 |
|   2015 |       7 | Minnesota  | East North Central | severe weather     |              1740 |               250000 |


### Univariate Analysis

This bar plot illustrates the number of power outages across each climate region, highlighting the `Northeast` region's significantly higher outage count compared to all others.

<iframe
  src="assets/univariate-plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This bar plot shows the number of power outages distributed by `cause category`. Severe weather and intentional attack outages are the two most common causes. 

<iframe
  src="assets/univariate-plot2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

As we saw previously, severe weather and intentional attacks are the two leading causes of power outages in the dataset. While the median electricity price for outages from both causes is similar, severe weather outages exhibit greater price variability. This suggests that the timing and location of severe weather-related outages may differ more widely.

<iframe
  src="assets/bivariate-plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This scatter plot depicts the relationship between demand loss and outage duration. Surprisingly, the data reveals that the longest outages did not necessarily result in the greatest demand loss accumulated. In fact, there are even a few outages with significant demand loss but relatively short durations. This suggests that other factors, such as climate region or cause category, may play a crucial role in determining how much total demand lost there is for a given outage.

<iframe
  src="assets/bivariate-plot2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

This table presents the mean values of key metrics for power outages, grouped by climate region. It emphasizes regional variations in outage duration, demand loss, and customers affected.

| climate_region     |   anomaly_level |   outage_duration |   demand_loss_mw |   customers_affected |
|:-------------------|----------------:|------------------:|-----------------:|---------------------:|
| Central            |     -0.166332   |          2701.13  |          477.482 |             126810   |
| East North Central |     -0.15942    |          5352.04  |          560.406 |             138389   |
| Northeast          |     -0.176218   |          2991.66  |          537.411 |             121960   |
| Northwest          |     -0.00681818 |          1284.5   |          177.897 |              81420   |
| South              |      0.0180617  |          2846.1   |          399.087 |             183501   |
| Southeast          |     -0.135333   |          2217.69  |          761.533 |             180540   |
| Southwest          |     -0.0467391  |          1566.14  |          424.556 |              39028   |
| West               |     -0.0285714  |          1628.33  |          651.457 |             194580   |
| West North Central |     -0.2375     |           696.562 |          326     |              47316   |

This pivot table summarizes the frequency of power outages by cause category across different climate regions, offering insights into the distribution of outage causes regionally.

| cause_category                |   Central |   East North Central |   Northeast |   Northwest |   South |   Southeast |   Southwest |   West |   West North Central |
|:------------------------------|----------:|---------------------:|------------:|------------:|--------:|------------:|------------:|-------:|---------------------:|
| equipment failure             |         7 |                    3 |           5 |           2 |      10 |           5 |           5 |     21 |                    1 |
| fuel supply emergency         |         4 |                    5 |          14 |           1 |       7 |           0 |           2 |     17 |                    1 |
| intentional attack            |        38 |                   20 |         135 |          89 |      28 |           9 |          64 |     31 |                    4 |
| islanding                     |         3 |                    1 |           1 |           5 |       2 |           0 |           1 |     28 |                    5 |
| public appeal                 |         2 |                    2 |           4 |           2 |      42 |           5 |           1 |      9 |                    2 |
| severe weather                |       135 |                  104 |         176 |          29 |     113 |         118 |          10 |     70 |                    4 |
| system operability disruption |        11 |                    3 |          15 |           4 |      27 |          16 |           9 |     41 |                    0 |

Expanding on the distribution of outage causes, this pivot table shows how these causes vary across different climate conditions.

| cause_category                |   cold |   normal |   warm |
|:------------------------------|-------:|---------:|-------:|
| equipment failure             |     19 |       28 |     10 |
| fuel supply emergency         |     19 |       26 |      5 |
| intentional attack            |    122 |      226 |     70 |
| islanding                     |     15 |       17 |     14 |
| public appeal                 |     22 |       34 |     13 |
| severe weather                |    239 |      354 |    166 |
| system operability disruption |     37 |       59 |     30 |


## Assessment of Missingness


## Hypothesis Testing

To understand the nature of outage durations further, we conducted a permutation test between outages that occurred during warm and cold climate episodes. The following hypotheses were tested at a 5% signficance level to determine if outage durations come from the same population:

- **Null Hypothesis**: Outage durations during warm climate episodes and cold climate episodes have the same distribution, and the observed differences in our samples are due to random chance.

- **Alternative Hypothesis**: Outage durations during warm climate episodes are different than outage durations during cold climate episodes, on average. The observed difference in our samples cannot be explained by random chance alone.

To clearly separate observations with uncommon appearances, we used a **test statistic** of *absolute difference in group means*, which is defined as:

$| \text{mean duration (warm climate)} - \text{mean duration (cold climate)} |$


This ensures we measure distances, such that large values of the test statistic correspond to one hypothesis, and small values correspond to the other. The absolute difference captures the magnitude of change in outage durations between warm and cold climate episodes, regardless of direction.

To simulate the permutation test, we run 10000 trials to repeatedly shuffle the durations, compute the means for each climate category, and calculate the test statistic. Each computed statistic is added to an array.

By comparing our array of statistics with our calculated observed statistic, we computed a p-value of 0.757. Since our p-value is over our significance threshold of 0.05, we fail to reject the null hypothesis as there seems to be no significant difference between outage durations during warm climate episodes and cold climate episodes.

The following histogram displays the empirical distribution from our simulation, showing that our observed statistic falls within the bounds of common values in the distribution.

<iframe
  src="assets/simulation-plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis
