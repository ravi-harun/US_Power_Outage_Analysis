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
