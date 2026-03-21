# Modeling Power Outages in America
A multistage assessment of a power outage data base tracking outages in the continental United States from January 2000 to July 2016
By Christian Ramirez

## Introduction
In this project I aim to explore factors that impact the duration of power outages. I feel it would be valuable to look into this question as power outage duration generally is among the most impactful effects of an outage. Understanding the processes affecting the duration of a power outage may provide tangible insights into how to properly address these outages and reduce severity.

The dataset utilized for this project was collected from various publicly available datasets and compiled into a single dataset with 1533 entries and 55 categories. These categories cover a spectrum of fields related power outages such as geographic, economic, time, demographic data. The dataset is cataloged by Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure at https://engineering.purdue.edu/LASCI/research-data/outages. 

For this project I worked with 11 categories for this project as follows:


| Column      | Description |
| ----------- | ----------- |
| `outage.start.date`| Date when outage event began    |
| `outage.start.time`| Time when outage event began    |
| `outage.restoration.date`| Date when said outage event had been restored       |
| `outage.restoration.time`| Time when said outage event had been restored       |
| `climate.category`| Represents the climate episodes corresponding to the years. Given as "warm", "normal", or "cold"        |
| `demand.loss.mw`  | Amount of peak demand lost during an outage event     |
| `climate.region`  | U.S. Climate regions as specified by National Centers for Environmental Information (9)   |
| `areapct_urban`   | Percentage of the land area of respective U.S. state represented by the land area of the urban clusters  |
| `nerc.region`     | The North American Electric Reliability Corporation (NERC) regions involved in the outage event (6)  |
| `cause.category`  | Categories of all the events causing the major power outages (6)|
| `outage.duration` | Duration of outage events (minutes)|

## Data Cleaning and Exploratory Data Analysis
The data set collected by the researchers was large, contained missing values, and provided information not necessary to this analysis. So, I began this analysis by cleaning the dataset.

### Cleaning

1. To begin cleaning I reformatted the dataset to utilize a numerical index and set the coloumn names. Then I removed unneccessary columns leaving only the 11 aformentioned columns.

2. Then I combined `outage.start.date` with `outage.start.time` and `outage.restoration.date` with `outage.restoration.time`. Both columns provided string values before but I chose to combine and change the type to utilize pd.timestamp values.

3. Next, I dropped about 21 rows that mostly contained outliers. This was to prevent prediction interference
    1. First I dropped 6 rows from `nerc.region` pertaining to 4 NERC Regions that only appear in the dataset less than 3 times. These regions only contributed 0.3% of the data.
    2. Next, I dropped 6 rows containing the largest values in the `outage.duration` column. All of these values reached far over 40,000 minutes when the median `outage.duration` value was onl 690 minutes.
    3. Finally, I dropped NaN values from the `outage.start.date` column as there were 9 of these rows


4. Next, I chose to drop 175 rows from the `outage.duration` column containing 0 and 1 minute values. I made this choice because these values not only impacted the effectiveness of the model but also because I felt that including outages events that seemingly lasted 0 or 1 minutes seemed rather unneccessary. I deduced that these values were not entirely accurate and had a larger negative impact on the analysis than benefit.

### Filling null values and Transforming

1. I filled null values in the `demand.loss.mw` columns. There was 702 null rows in this column and I chose to fill these rows using probabilistic imputation, where values were randomly drawn from the distribution of non-null values to complete the data set without changing the distribution much.

2. Then I observed that the range of `outage.duration` values varied widely and would likely be modelled best after a log transformation. This choice not only linearized the data better but also dispersed the data in a manner that allowed for better visual displays.

The result was a dataset with 10 columns and 1337 rows. These are the first 5 rows of the dataframe containing the data:

| u.s._state   | nerc.region   | climate.region     | climate.category   | outage.start.date   | outage.restoration.date   | cause.category     |   outage.duration |   demand.loss.mw |   areapct_urban |
|:-------------|:--------------|:-------------------|:-------------------|:--------------------|:--------------------------|:-------------------|------------------:|-----------------:|----------------:|
| Minnesota    | MRO           | East North Central | normal             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00       | severe weather     |           8.02617 |                0 |            2.14 |
| Minnesota    | MRO           | East North Central | cold               | 2010-10-26 20:00:00 | 2010-10-28 22:00:00       | severe weather     |           8.00637 |             1000 |            2.14 |
| Minnesota    | MRO           | East North Central | normal             | 2012-06-19 04:30:00 | 2012-06-20 23:00:00       | severe weather     |           7.84385 |              300 |            2.14 |
| Minnesota    | MRO           | East North Central | warm               | 2015-07-18 02:00:00 | 2015-07-19 07:00:00       | severe weather     |           7.46164 |              250 |            2.14 |
| Minnesota    | MRO           | East North Central | cold               | 2010-11-13 15:00:00 | 2010-11-14 22:00:00       | severe weather     |           7.52833 |              266 |            2.14 |

### Exploratory Data Analysis

I continued intial assessment of the dataset by performing *Univariate* and *Bivariate* data analysis

#### Univariate Analysis

First I began by graphing the *median* duration of outage events by US State

<iframe
  src="assets/map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Then I assessed Outage Duration (transformed by log) by Date

<iframe
  src="assets/date_duration2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Then I assessed the amount of Outage Events per Cause Category

<iframe
  src="assets/bar3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Bivariate Analysis

I continued analysis with Bivariate Analysis, beginning with an assessment of Outage Duration (transformed by log) by Cause Category.
I included a line plot connecting the median Outage Duration value per each Cause Category

<iframe
  src="assets/cause_duration4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, I plotted the Outage Duration by Demand Loss with both axes transformed by log.
It's also important to note that this graph contains filled in null values for Demand Loss, filled by probabilistic imputation.
It's best understood as a visualization of distribution but not necessarily an exact representation. 

<iframe
  src="assets/demand_loss5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Lastly, I created a plot of Outage Duration (transformed by log) by NERC Region.
And again for this plot, I included a line plot connecting the median Outage Duration value per each NERC Region.

<iframe
  src="assets/nerc_duration6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Assessment of Missingness

### MNAR Analysis

I believe the `demand.loss.mw` column's 702 missing values can be attributed to Missing Not At Random (MNAR) missingness. This data was obtained from mutliple sources with may have varying methods for recording Demand Loss. Very high or small demand loss may be difficult to determine or report. During these instances demand loss was likely difficult to precisely measure which may have caused the lack of reporting. Additionally, the need to retrieve Demand Loss data from multiple sources may indicate that this measure is simply very difficult to record in general, hence the lack of reporting entities. In total, Demand Loss was highely unrecorded within this dataset likely due to the nature of measuring Demand Loss itself.

### Missingness Dependency

For this next section I performed permutation tests and measured the Total Variation Distance between the proportion of null values in the `customers.affected` column with respect to other columns. This was done with the aim to determine if the nullness from the `customers.affected` column was dependent on another column in the dataframe or not. This would effectively attributing the missingness of `customers.affected` to Missing At Random (MAR).

Through this investigation I determined the missingness of the `customers.affected` column was dependent on NERC Region. The results of this permutation test are shown in the histogram below. The observed TVD between the null values and non-null values in `customers.affected` and `nerc.regions` column was **~0.266**. After the 500 permutation shuffles and calculating the test TVDs it was found that this observed statistic was signficantly unlikely to have occured by chance as the resulting p-value was **~0.0**

<iframe
  src="assets/TVDnerc1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Additionally, I also performed this permutation test between the null values and non-null values in `customers.affected` and `climate.regions`. This test showed an observed TVD value of **~0.0363**. After permutation shuffling and calculating another 500 test TVDs a p-value of **~0.692** was found. Thus indicating that the missiningness of `customers.affected` *is not* dependent on the `climate.regions` data, accepting the null hypothesis in this case.

<iframe
  src="assets/TVDclimate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

Lastly, I performed a hypothesis test to assess these hypotheses the average time of Outage Duration with respect to Climate Categories. The null and alternative hypothesis were as follows:

> Null hypothesis: Outage Duration on average is the same for 'Warm' and 'Cold' climate categories

> Alternate hypothesis: Outage Duration on average is the larger for 'Warm' when compared to 'Cold' climate categories

To perform this hypothesis test I employed a absolute difference of means as a test statistic (Difference between 'Warm' and 'Cold') permutation shuffling. The observed test statistic was **177.679** and the resulting p-value after testing was **~0.654**. This allows the null hypothesis to be accepted indicating that it is likely the case that Outage Duration is the same for 'Warm' and 'Cold' climate categories on average.

<iframe
  src="assets/TVDhyp3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Framing a Prediction Problem

For this dataset, I intended to predict Outage Duration for outage events in the continential United States. I feel the insight gained from predicting the duration of an outage event may significantly reduct the impact of these random events. The features used for these prediction models were: `month`, `climate.category`, `climate.region`, `areapct_urban`, `nerc.region`, `cause.category`, and the predictor column: `outage.duration`. I chose these as they seemed to have at least a slight correlation with Outage Duration, as assessed through modelling and multiple iterations of models. Of these `areapct_urban` was the only quantitative feature. The rest are all nominal features, while `outage.duration` is what we are trying to predict and is qualitative.

For this process I used the Sklearn package to utilize column transformers, pipelines, and their built in Linear Regression functions. Because I was attempting to predict a quanititative value, I opted to use a Linear Model trained through k-folds training. 

## Baseline Model

For the baseline model I did not utilize k-folds training to optimize the model. I opted to stick to only two features initially, `nerc.region` and `cause.category`. For these features I One Hot Encoded the columns and fitted a Linear Regression model to the resulting dataframe of 2 features. This starting model achieved a R^2 score of **~0.175**

## Final Model

To expand the model I began by including all 6 features aforementioned above. Then I One Hot Encoded all the nominal features. At this point it explored multiple transformations and pipelines to assess which one performed the best under 15 k-validation folds. It was at this point in my exploration where I discovered that applying a logarithmic scale to the Outage Duration data allowed for better predictions under a Linear model. Additionally, I tested varying amounts of features as well as different binarizing thresholds on `areapct_urban`. The training data showed that using all columns and applying a Binarizing mask to the `areapct_urban` features with a threshold 6 provided the best predictions on average. Additionally, I attempted to use Least Absolute Shrinkage and Selection Operator (Lasso) regression to model, with varying regularization strength. However, k-fold testing revealed that this method was worse than Linear Regression in all cases.

Thus, the final model utilized Linear Regression on 5 One Hot Encoded, nominal features, with a binarized `areapct_urban` feature utilizing a threshold of 6. Addtionally, the `areapct_urban` was standardized while the `outage.duration` prediction data was transformed under a log scale. This model scored on average an R^2 score of **~0.423**