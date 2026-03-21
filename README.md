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

1. I filled null values in the `demand.loss.mw` columns. There was 702 null rows in this column and I chose to fill these rows using probabilistic imputation, where I randomly drew values from the distribution of non-null values to complete the data set without changing the distribution much.

2. Then I observed that the range of `outage.duration` values varied widely and would likely be modelled best after a log transformation. This choice not only linearized the data better but also dispersed the data in a manner that allowed for better visual displays.

The result was a dataset with 10 columns and 1337 rows. These are the first 10 rows of the dataframe containing the data:

'|    | u.s._state   | nerc.region   | climate.region     | climate.category   | outage.start.date   | outage.restoration.date   | cause.category     |   outage.duration |   demand.loss.mw |   areapct_urban |\n|---:|:-------------|:--------------|:-------------------|:-------------------|:--------------------|:--------------------------|:-------------------|------------------:|-----------------:|----------------:|\n|  0 | Minnesota    | MRO           | East North Central | normal             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00       | severe weather     |           8.02617 |                0 |            2.14 |\n|  1 | Minnesota    | MRO           | East North Central | cold               | 2010-10-26 20:00:00 | 2010-10-28 22:00:00       | severe weather     |           8.00637 |             1000 |            2.14 |\n|  2 | Minnesota    | MRO           | East North Central | normal             | 2012-06-19 04:30:00 | 2012-06-20 23:00:00       | severe weather     |           7.84385 |              300 |            2.14 |\n|  3 | Minnesota    | MRO           | East North Central | warm               | 2015-07-18 02:00:00 | 2015-07-19 07:00:00       | severe weather     |           7.46164 |              250 |            2.14 |\n|  4 | Minnesota    | MRO           | East North Central | cold               | 2010-11-13 15:00:00 | 2010-11-14 22:00:00       | severe weather     |           7.52833 |              266 |            2.14 |\n|  5 | Minnesota    | MRO           | East North Central | cold               | 2010-07-17 20:30:00 | 2010-07-19 22:00:00       | severe weather     |           7.99632 |                0 |            2.14 |\n|  6 | Minnesota    | MRO           | East North Central | normal             | 2005-06-08 04:00:00 | 2005-06-10 22:00:00       | severe weather     |           8.284   |               75 |            2.14 |\n|  7 | Minnesota    | MRO           | East North Central | warm               | 2015-03-16 07:31:00 | 2015-03-16 10:06:00       | intentional attack |           5.04343 |               20 |            2.14 |\n|  8 | Minnesota    | MRO           | East North Central | normal             | 2013-06-21 17:39:00 | 2013-06-24 06:00:00       | severe weather     |           8.19451 |                0 |            2.14 |\n|  9 | Minnesota    | MRO           | East North Central | normal             | 2013-06-21 03:00:00 | 2013-06-26 12:00:00       | severe weather     |           8.95416 |              168 |            2.14 |'

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

Then I assessed Outage Duration by Date
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
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis