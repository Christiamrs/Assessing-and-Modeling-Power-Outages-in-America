# Modeling Power Outages in America
A multistage assessment of a power outage data base tracking outages in the continental United States from January 2000 to July 2016
By Christian Ramirez

## Introduction
Provide an introduction to your dataset, and clearly state the one question your project is centered around. 
Why should readers of your website care about the dataset and your question specifically? 
Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns

In this project I aim to explore factors that impact the duration of power outages. I feel it would be valuable to look into this question as power outage duration generally is among the most impactful effects of an outage. Understanding the processes affecting the duration of a power outage may provide tangible insights into how to properly address these outages and reduce severity.

The dataset utilized for this project was collected from various publicly available datasets and compiled into a single dataset with 1533 entries and 55 categories. These categories cover a spectrum of fields related power outages such as geographic, economic, time, demographic data. The dataset is cataloged by Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure at https://engineering.purdue.edu/LASCI/research-data/outages. 
For this project I worked with 11 categories for this project as follows:


| Column      | Description |
| ----------- | ----------- |
| `outage.start.date`| Date when outage event began    |
| `outage.start.time`| Time when outage event began    |
| `outage.restoration.date`| Date when said power outage had been restored       |
| `outage.restoration.time`| Time when said power outage had been restored       |
| `climate.category`| Represents the climate episodes corresponding to the years. Given as "warm", "normal", or "cold"        |
| `demand.loss.mw`  | Amount of peak demand lost during an outage event     |
| `climate.region`  | U.S. Climate regions as specified by National Centers for Environmental Information (9)   |
| `areapct_urban`   | Percentage of the land area of the U.S. state represented by the land area of the urban clusters  |
| `nerc.region`     | The North American Electric Reliability Corporation (NERC) regions involved in the outage event (6)  |
| `cause.category`  | Categories of all the events causing the major power outages (6)|
| `outage.duration` | Duration of outage events (minutes)|

## Data Cleaning and Exploratory Data Analysis
## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis