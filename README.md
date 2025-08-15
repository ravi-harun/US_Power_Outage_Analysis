# A Data Analysis of Major U.S. Power Outages 
*<I> This project received a 100/100 from DSC 80's Fall 2024 Offering at UC San Diego. <br>
**Authors**
- Michael Tang (mjtang@ucsd.edu)
- Ravi Harun (rharun@ucsd.edu)

## Introduction

Power outages disrupt the daily lives of millions and result in billions of dollars in economic losses annually, particularly in regions prone to extreme weather events <a name="cite_ref-1"></a>[<sup>1</sup>](#cite_note-1). Beyond the immediate inconvenience of losing electricity, these outages often have far-reaching effects on public safety, health services, and economic stability.

This analysis investigates: *What factors have the most influence on the cause and duration of a power outage?* By leveraging historical outage data from January 2000 to July 2016 across the United States, we aim to uncover trends, identify key drivers of prolonged outages, and offer insights into mitigation strategies. Understanding these patterns can inform more resilient infrastructure planning and targeted interventions to minimize disruption.

The data used for our analysis is sourced from [Purdue University](https://engineering.purdue.edu/LASCI/research-data/outages). This raw dataset contains 1534 rows.

The table below contains a data dictionary <a name="cite_ref-2"></a>[<sup>2</sup>](#cite_note-2) on a subset of relevant columns utilized for our analysis (referenced in cleaned snake case column names).

|Variable name|Description|
|---|---|
|`'year'`|Indicates the year when the outage event occurred|
|`'month'`|Indicates the month when the outage event occurred| 
|`'us_state'`|Represents all the states in the continental U.S.|
|`'postal_code'`|Represents the postal code of the U.S. states|
|`'nerc_region'`|The North American Electric Reliability Corporation (NERC) regions involved in the outage event|
|`'climate_region'`|U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)|
|`'anomaly_level'`|This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season|
|`'climate_category'`|This represents the climate episodes corresponding to the years|
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

The raw dataset did not require much data cleaning to be used. 

After reading the raw data into a Pandas DataFrame, we dropped extra columns such as `variables` and `OBS` that contained no meaningful information.

We also converted the column names to snake case, making them easier to reference when performing data wrangling and EDA. 

A portion of selected columns from the cleaned DataFrame is displayed below.

|   year |   month | us_state   | climate_region     | cause_category     |   outage_duration |   customers_affected |
|-------:|--------:|:-----------|:-------------------|:-------------------|------------------:|---------------------:|
|   2011 |       7 | Minnesota  | East North Central | severe weather     |              3060 |                70000 |
|   2014 |       5 | Minnesota  | East North Central | intentional attack |                 1 |                  nan |
|   2010 |      10 | Minnesota  | East North Central | severe weather     |              3000 |                70000 |
|   2012 |       6 | Minnesota  | East North Central | severe weather     |              2550 |                68200 |
|   2015 |       7 | Minnesota  | East North Central | severe weather     |              1740 |               250000 |


### Univariate Analysis

First, we investigated the number of power outages across each climate region. This bar plot highlights the `Northeast` region's significantly higher outage count compared to all others.

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

The heatmap below illustrates the number of outages across states, with yellow indicating fewer outages and red indicating higher numbers. California stands out with the highest number of outages compared to other states.

<iframe
  src="assets/outages_by_state.html"
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

This table presents the mean values of key metrics for power outages, grouped by climate region. It emphasizes regional variations in outage duration, demand loss, and customers affected. Interestingly, `South` is the only region with a positive anomaly level.

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

This pivot table summarizes the frequency of power outages by cause_category across different climate regions, offering insights into the distribution of outage causes regionally. While most regions are affected by severe weather the most, this is not the case for regions like `Northwest` and `Southwest`, as intentional attacks are the most frequent causes.

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

### NMAR Analysis

Our dataset contains several columns with missing values. Amongst those, the `demand_loss_mw` column is likely to be Not Missing At Random (NMAR), such that the missing values in that column depends on the actual missing values. This is because the missingness is likely due to the data collection method, where certain utility companies may choose not to report the amount of demand lost during an outage due to several reasons such as measurement challenges or minor outages. 

If we had access to the outage reports of each utility company responsible for electricity in a given region, we could gain further insight into the missingness mechanism and determine whether `demand_loss_mw` is actually MAR. Then, we would run an analysis on whether the missingness of demand lost reported is dependent on the company.

### Missingness Dependency

Given the valuable information that the `outage_duration` column provides, we believe that it is important to test its **MAR** dependency.
This is defined as the chance that a value is missing *depends on other columns*, but *not* the actual missing value itself. 
We chose two columns, `climate_region` and `month`, and conducted a permutation test on each at a 5% significance level to determine statistical significancy. 

Since both columns are categorical, we utilized a Total Variation Distance (TVD) test statistic to measure the distance between two categorical distributions.

#### Climate Region

- **Null Hypothesis**: The distribution of `climate_region` is the same when `outage_duration` is missing and not missing.
- **Alternative Hypothesis**: The distribution of `climate_region` is different when `outage_duration` is missing and not missing.

The proportion of the missingness is shown below.

<iframe
  src="assets/climate_reg_bar_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The empirical distribution is shown below. We obtained an observed TVD of 0.25 and a p-value of **0.006**. Since this p-value is statistically significant, we **reject** the null hypothesis in favor of the alternative hypothesis and conclude that `outage_duration` is likely MAR dependent on `climate_region`. 

<iframe
  src="assets/climate_reg_empirical_dist_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Month

- **Null Hypothesis**: The distribution of `month` is the same when `outage_duration` is missing and not missing.
- **Alternative Hypothesis**: The distribution of `month` is different when `outage_duration` is missing and not missing.

The proportion of the missingness is shown below.

<iframe
  src="assets/month_bar_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The empirical distribution is shown below. We obtained an observed TVD of 0.22 and a p-value of **0.112**. Since this p-value is statistically insignificant, we **fail to reject** the null hypothesis and conclude that `outage_duration` is not likely to be MAR dependent on `month`. 

<iframe
  src="assets/month_empirical_dist_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

To understand the nature of outage durations further, we conducted a permutation test between outages that occurred during warm and cold climate episodes. The following hypotheses were tested at a 5% signficance level to determine if outage durations come from the same population:

- **Null Hypothesis**: Outage durations during warm climate episodes and cold climate episodes have the same distribution, and the observed differences in our samples are due to random chance.

- **Alternative Hypothesis**: Outage durations during warm climate episodes are different than outage durations during cold climate episodes, on average. The observed difference in our samples cannot be explained by random chance alone.

To clearly separate observations with uncommon appearances, we used a **test statistic** of *absolute difference in group means*, which is defined as:

| mean duration (warm climate) - mean duration (cold climate) |

This ensures we measure distances, such that large values of the test statistic correspond to one hypothesis, and small values correspond to the other. The absolute difference captures the magnitude of change in outage durations between warm and cold climate episodes, regardless of direction.

To simulate the permutation test, we run 10000 trials to repeatedly shuffle the durations, compute the means for each climate category, and calculate the test statistic. Each computed statistic is added to an array.

By comparing our array of statistics with our calculated observed statistic, we computed a p-value of 0.757. Since our p-value is over our significance threshold of 0.05, we fail to reject the null hypothesis as there seems to be no significant difference between outage durations during warm climate episodes and cold climate episodes.

The following histogram displays the empirical distribution from our simulation, showing that our observed statistic falls within the bounds of common values in the distribution.

<iframe
  src="assets/simulation-plot1.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

Our objective is to predict the cause behind major power outages (`cause_category`) to identify key contributing factors and support preventative measures. This is a multiclass classification problem, as outages can result from various factors. We evaluate our classification model using accuracy as the performance metric, chosen for its simplicity and effectiveness in assessing overall correctness in this context. To ensure real-world applicability and avoid data leakage, we only included features that are accessible before or as an outage is occurring, such as `climate_region` and `population`. Features that measure the severity of an outage like `outage_duration` or `customers_affected`, which are only known after an outage occurs, are excluded from the model.

## Baseline Model

For our baseline model, we utilize a random forest classifier with two features: `us_state` and `total_price`. The `us_state` feature, a categorical nominal variable, is transformed into a set of binary features through one-hot encoding to make it compatible with the model. In contrast, `total_price` is already a quantitative variable and does not require any transformation.

The baseline model achieves an accuracy of 0.603 on the test set, indicating a moderate performance in predicting the cause category of a power outage. Considering the complexity of factors influencing power outage causes, this result is reasonable given the model's reliance on only two features.

## Final Model

The final model builds upon the baseline by incorporating three additional features: `month`, `climate_region`, and `population`. These features were selected based on their relevance to the data-generating process and their potential to improve predictive performance.

`month` is an ordinal variable that can capture seasonality, which is crucial because power outages often correlate with seasonal factors such as extreme weather events (e.g., hurricanes, heatwaves, or winter storms). Including this feature allows the model to account for monthly variations in outage causes.

`climate_region` is a categorical nominal variable that can provide regional context, highlighting differences in environmental and infrastructural factors across various parts of the United States. For instance, some regions may be more prone to outages caused by hurricanes, while others may face risks from snowstorms.

`population` is a quantitative variable that can reflect the electricity consumption based on the number of people in a region. Areas with larger populations may experience different outage patterns compared to less densely populated areas.

By combining these features with `us_state` and `total_price` from the baseline model, the model leverages both local and broader contextual information to better predict the cause categories of power outages.

The final model also employs a random forest classifier, selected for its ability to handle a mix of categorical and continuous variables, robustness to feature scaling, and resistance to overfitting when appropriately tuned. 

To optimize performance, GridSearchCV was used to perform hyperparameter tuning, with the best hyperparameters found to be:

- `max_depth: 12` – Limits the depth of each tree to prevent overfitting while retaining sufficient complexity to capture patterns.

- `min_samples_split: 15` – Ensures that splits occur only if a node contains at least 15 samples, reducing the likelihood of creating overly specific branches.

- `n_estimators: 300` – Utilizes 300 trees to enhance the stability and accuracy of predictions.

The final model achieves an accuracy of 0.660 on the test set, an improvement over the baseline model's accuracy of 0.603. This performance boost is attributed to the inclusion of additional features that capture critical aspects of the data-generating process, such as seasonal patterns, regional differences, and population-driven demand.

## Fairness Analysis

We assess the fairness of our model by performing a fairness analysis on two groups:

- Small populations, defined as populations less than 13,000,000
- Large populations, defined as populations greater than 13,000,000

The threshold between the two groups is chosen based on the mean number of `population` for power outages in our dataset.

We compare the cause category predicted by our model across the two groups as a measure of how our model can classify severity of an outage. We used the accuracy evaluation metric in testing the following hypotheses at a 5% signficance level to determine if our classifier achieves accuracy parity:

- **Null Hypothesis**: Our model is fair. Its accuracy for small populations and large populations are roughly the same, and any differences are due to random chance.

- **Alternative Hypothesis**: Our model is unfair. Its accuracy for small populations is significantly different than its accuracy for large populations.

To clearly separate observations with uncommon appearances, we used a **test statistic** of *absolute difference in group means*, which is defined as:

| mean accuracy score (small populations) - mean accuracy score (large populations) |

We transform the `population` column using `Binarizer` to classify observations into binary classes based on the threshold of a population size of 13,000,000, creating a new column called `bi_population`. 
To simulate the permutation test, we run 1000 trials to repeatedly shuffle `bi_population`. We calculate the test statistic, the accuracy score for our model's predicted cause category and actual cause category.

The p-value from our permutation test is **0.0**, indicating a statistically significant result under a 5% significance level. Therefore, we reject the null hypothesis that our model is fair. The model performs differently for small populations of less than 13,000,000 people and large populations of more than 13,000,000 people.

The following histogram displays the empirical distribution from our simulation, showing that our observed statistic is not a commonly observed value in the distribution.

<iframe
  src="assets/simulation-plot2.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

This disparity suggests that the model may be biased toward certain characteristics associated with the severity of outages, such as demand loss and outage duration. Such bias could result in inequitable resource allocation or decision-making in real-world applications, potentially leaving outages in smaller communities under-prioritized. To address this issue, further refinement of the model should incorporate fairness considerations across different groups, ensuring more equitable outcomes and enhancing the model's reliability and social impact.


## References

1. <a name="cite_note-1"></a> [](#cite_ref-1) Mukherjee, S., Nateghi, R., & Hastak, M. (2018). A multi-hazard approach to assess severe weather-induced major power outage risks in the U.S. Reliability Engineering & System Safety, 175, 283–305. https://doi.org/10.1016/j.ress.2018.03.015
2. <a name="cite_note-2"></a> [](#cite_ref-2) Mukherjee, S., Nateghi, R., & Hastak, M. (2018). Data on major power outage events in the continental U.S. Data in Brief, 19, 2079–2083. https://doi.org/10.1016/j.dib.2018.06.067
