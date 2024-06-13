# PERFORMING POWER OUTAGE ANALYSIS
Project for DSC80 at UCSD

By Berlen Zhang and Sohan Raval

## INTRODUCTION
This project aims to analyze what factors have the greatest impact on the severity of power outages. In our work, we answered the question: do different regions and causes affect the severity of power outages in the US? Additionally, we developed a predictive model that forecasts the duration of power outages given a set of details of the outage. This will be a useful tool that allows utility companies to enhance communication with customers when an outage occurs. Being able to inform the public about how long an outage is predicted to last will help in managing and mitigating the consequences of power outages.


The dataset we used to generate an answer to our project question contains details of 1535 power outages in the US, each outage being represented by a row. It includes 57 columns containing specific information on the outage. We only used 24 of these columns in our project.


Below is a description of all the columns we will be working with:

- `YEAR`: The year a power outage took place.

- `MONTH` : The month a power outage took place.

- `U.S._STATE`: The state where the power outage took place.

- `POSTAL.CODE`:The abbreviation of the state column.

- `NERC.REGION`: The region in which the North American Electric Reliability Corporation was present in response to the outage.

- `CLIMATE.REGION`: 9 regions consistent with the climate as produced by the National Centers for Environmental Information(NCEI) scientists.

- `CLIMATE.CATEGORY`: The state of the Climate when the outage occurred.

- `CAUSE.CATEGORY`: The cause of the outage.

- `CAUSE.CATEGORY.DETAIL`: More in-depth information about the cause of the outage.

- `OUTAGE.DURATION`: How long the outage lasted in minutes

- `DEMAND.LOSS.MW`: The demand loss by megawatt

- `CUSTOMERS.AFFECTED`: The amount of customers affected by the outage

- `TOTAL.SALES`: The amount of energy consumed by megawatt per hour

- `PC.REALGSP.STATE`: The economic output of a state at the time of the outage.

- `POPULATION`: Population of the area affected by the outage

- `POPDEN_URBAN`: The population density of urban areas in a specified state.

- `POPDEN_RURAL`:  The population density of rural areas in a specified state.

- `OUTAGE.START`: A timestamp of when the outage started

- `OUTAGE.RESTORATION`: A timestamp of when the outage ended

---
## DATA CLEANING AND EXPLORATORY ANALYSIS

### Data Cleaning

We cleaned the data to better fit our objectives with the following steps:
1. Edit data frame to only contain the columns we are interested in utilizing in our project:
   * We dropped columns that would be unnecessary for predicting the duration of a power outage.        The 24 columns displayed above are the ones we decided would be useful for the project.
3. Replace the zero values in columns `CUSTOMERS.AFFECTED` and `OUTAGE.DURATION` with nan:
   * It would be unrealistic for an outage to have a duration of zero since this would mean there       was no outage at all. It is also impossible for there to be zero customers affected since          this would signify that no one was there to witness or report the power outage.
4. Converting columns `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `POPDEN_URBAN`, `CUSTOMERS.AFFECTED`, `TOTAL.SALES`, `PC.REALGSP.STATE`, `POPDEN.RURAL` into suitable data types:
   * The data in the columns were originally string values. In order to use them in the             necessary data analysis procedures we would conduct, we had to convert them into float values.
4. Combining columns `OUTAGE.START.DATE` and  `OUTAGE.START.TIME` into a column named                 `OUTAGE.START`:
   * Originally these columns contained string type values. We combined them into a column and          converted them into timestamp objects, making the data more convenient to use in calculations.
5. Combining columns `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into a column named      `OUTAGE.RESTORATION`:
   * Originally these columns contained string type values. We combined them into a column and          converted them into timestamp objects, making the data more convenient to use      in calculations.
6. Replacing all the nans in the `CLIMATE.REGION` column with the text 'No Climate Region'
   * The missing values in this column were missing by design.
7. Filtered out Alaska
   * Alaska was severly underepresented in the dataset. The only instance of Alaska in the dataset had missing values in many of the other colummns.

Shown below is the first five rows of the cleaned version of the dataframe:

|   MONTH |   YEAR | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.PRICE |   TOTAL.SALES |   TOTAL.CUSTOMERS |   PC.REALGSP.STATE |   PC.REALGSP.USA |   TOTAL.REALGSP |   POPULATION |   POPDEN_URBAN |   POPDEN_RURAL | OUTAGE.START        | OUTAGE.RESTORATION   |
|--------:|-------:|:-------------|:--------------|:--------------|:-------------------|:-------------------|:-------------------|:------------------------|------------------:|-----------------:|---------------------:|--------------:|--------------:|------------------:|-------------------:|-----------------:|----------------:|-------------:|---------------:|---------------:|:--------------------|:---------------------|
|       7 |   2011 | Minnesota    | MN            | MRO           | East North Central | normal             | severe weather     | nan                     |              3060 |              nan |                70000 |          9.28 |   6.56252e+06 |       2.5957e+06  |              51268 |            47586 |          274182 |  5.34812e+06 |           2279 |           18.2 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|       5 |   2014 | Minnesota    | MN            | MRO           | East North Central | normal             | intentional attack | vandalism               |                 1 |              nan |                  nan |          9.28 |   5.28423e+06 |       2.64074e+06 |              53499 |            49091 |          291955 |  5.45712e+06 |           2279 |           18.2 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|      10 |   2010 | Minnesota    | MN            | MRO           | East North Central | cold               | severe weather     | heavy wind              |              3000 |              nan |                70000 |          8.15 |   5.22212e+06 |       2.58690e+06 |              50447 |            47287 |          267895 |  5.3109e+06  |           2279 |           18.2 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|       6 |   2012 | Minnesota    | MN            | MRO           | East North Central | normal             | severe weather     | thunderstorm            |              2550 |              nan |                68200 |          9.19 |   5.78706e+06 |       2.60681e+06 |              51598 |            48156 |          277627 |  5.38044e+06 |           2279 |           18.2 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|       7 |   2015 | Minnesota    | MN            | MRO           | East North Central | warm               | severe weather     | nan                     |              1740 |              250 |               250000 |         10.43 |   5.97034e+06 |       2.67353e+06 |              54431 |            49844 |          292023 |  5.48959e+06 |           2279 |           18.2 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |


### Univariate Data Analysis

To start our project, we wanted to better understand where and when power outages were likely to happen. This would get us an idea of what factors contribute to the occurrence of outages. 

To visualize when power outages take place most, we created the histogram below, which displays the frequency of outages in the dataset for each month:

<iframe
  src="assets/outages-per-month.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To visualize where power outages take place most, we created the pie chart below, which displays the frequency of outages in the dataset for each climate region:

<iframe
  src="assets/climate-region-pie.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Data Analysis

Since our eventual goal is to build a model that predicts the duration of power outages. We wanted to analyze what factors have a strong influence on outage duration.

To observe the relationship between population and outage duration, we created a scatterplot between the `POPULATION` and `OUTAGE.DURATION` columns:

<iframe
  src="assets/population-by-duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To observe how outage duration differs in different parts of the US, we created an interactive choropleth map that displays the average outage duration in each state:

<iframe
  src="assets/mean-duration-map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To observe which outage causes result in the longest outage times, we created a histogram that displays the average outage duration for each cause category:

<iframe
  src="assets/duration-by-cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Examining Aggregate Statistics

In addition to visualizing data trends through plots, we also used dataset aggregation techniques to observe the features that have a large impact on the severity of an outage.

The first data frame we produced displays the mean outage duration and customers affected for each cause category. We chose to focus on the columns `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` because these metrics are a good representation of the severity of a power outage. This data frame is shown below:

| CAUSE.CATEGORY                |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |
|:------------------------------|------------------:|---------------------:|
| equipment failure             |          1850.56  |            105451    |
| fuel supply emergency         |         13484     |                 1    |
| intentional attack            |           521.934 |             18753.4  |
| islanding                     |           200.545 |              7232.72 |
| public appeal                 |          1468.45  |             15999.4  |
| severe weather                |          3899.71  |            190972    |
| system operability disruption |           747.092 |            211066    |

---
## ASSESSMENT OF MISSNGNESS

In this part of our analysis we look at the missing values in our dataset for columns `RESTORATION.TIME` and `DEMAND.LOSS.MW`


### NMAR ANALYSIS

We believe that the `RESTORATION.TIME` column is likely NMAR. If the restoration time of an outage is late at night, there is a greater chance of it being missing since the services that keep track of outage information may be unavailable at that time. Additional data we would obtain to explain the missingness of `RESTORATION.TIME` would be the city in which the outage occurred. We believe certain cities may have less reliable data recording services. Additionally, in order to find out if `RESTORATION.TIME` is Missing at Random(MAR), we can possibly look at which companies are responsible for the power outages in that particular city to check if their recording services are reliable, consequently determining if the missingness is dependent on the company’s recording services.

### MISSINGNESS DEPENDENCY

To figure out Missingness dependency we will conduct permutation tests on the `DEMAND.LOSS.MW` column based on the columns `CAUSE.CATEGORY` and `CLIMATE.CATEGORY`. In both permutation tests we used the Total Variation Distance(TVD) test statistic and used a 0.05 significance level.

*PERMUTATION TEST FOR CAUSE.CATEGORY:*

***Null Hypothesis*** : The distribution of the data in the column `CAUSE.CATEGORY` column is the same regardless of the missingness of the column `CAUSE.CATEGORY.DETAIL`. 

***Alternative Hypothesis*** : The distribution of data in the column `CAUSE.CATEGORY` is different depending on the missingess of the column `DEMAND.LOSS.MW`.

***Results*** : After conducting the permutation test, we found a TVD observed **test statistic of 0.179** and a **p-value of 0.0**, as a result, we reject the null hypothesis. The graph below shows the empirical distribution of this test which indicates that the `DEMAND.LOSS.MW` missingness is likely dependent on the `CAUSE.CATEGORY` column. The **red line** on the graph represents the observed test statistic.

<iframe
  src="assets/emp-dist-1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

*PERMUTATION TEST FOR CLIMATE.CATEGORY:*

***Null Hypothesis*** : The distribution of the data in the column `CLIMATE.CATEGORY` is the same regardless of the missingess of the column `DEMAND.LOSS.MW`.

***Alternative Hypothesis*** : The distribution of the data in the column `CLIMATE.CATEGORY` is different depending on the missingness of the column `DEMAND.LOSS.MW`.

***Results*** : For the second permutation test, we found a TVD observed **test statistic of 0.034** and a **p-value of 0.314**, therefore, we fail to reject the null hypothesis which indicates that the missing values of the `DEMAND.LOSS.MW` column likely does not depend on the `CLIMATE.CATEGORY` column. The graph below is the empirical distribution of this test and the **red line** on the graph represents the observed test statstic.

<iframe
  src="assets/emp-dist-2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
## HYPOTHESIS TESTING
Based on our central question we wanted to better analyze the regions on a larger scale so for this hypothesis test we looked at the distributions of the east and west regions. Although there are no columns that have this data, we used the data from the `CLIMATE.REGION` column to evaluate the distinction between the two regions based on the average number of `CUSTOMERS.AFFECTED`by power outages.

***Null Hypothesis*** : The mean amount of customers affected by power outages are the same in Eastern and Western climate regions.

***Alternative Hypothesis*** : The mean amount of customers affected by power outages in the Western climate region are greater than the Eastern climate region. 

***Test Statistic*** : Difference in means between the word ‘west’ and ‘east’, contained in the `CLIMATE.REGION` column.

***Significance level*** : 0.05

*PERMUTATION TEST*

For this hypothesis test we conducted a permutation test. In order to do this we first found the observed test statistic using Difference in means. To achieve this, we obtained all the values in the `ClMATE.REGION’ column that contains the string ‘west’ to find the average number of customers affected by the outage in the west region. We then did that for the string ‘east’ and found the **observed mean difference of 13,795.49**.

*RESULTS*

For the permutation test we conducted **1000 simulations** of the test statistic and got a **p-value of 0.286**. Due to the significance level of 0.05, we fail to reject the null hypothesis, meaning their is nothing to suggest that the average customers affected in the Western climate regions is greater than the Eastern climate regions.

Below is the Empirical Distribution of our permutation test.

<iframe
  src="assets/emp-dist-3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
## FRAMING A PREDICTION PROBLEM

The prediction problem we aim to focus on for this project is to build a model that can accurately predict the outage duration using multiple linear regression. Specifically, we plan on predicting values in the `OUTAGE.DURATION` column using other features in the dataset. We chose this response variable because being able to predict the duration of an outage can greatly reduce the negative impacts of a power outage. We believe this would be a valuable asset to companies and those who may be affected by outages. The metric we chose to evaluate our model is Root Mean Squared Error(RMSE) because RMSE is a reliable measurement to test the accuracy of regression models. 

We will only use features that would realistically be accessible during or before an outage. For example, the data in the column `OUTAGE.RESTORATION` would not be available to us during the time of the outage, therefore we will not use it as a feature in our predictive model. The features we plan on using are the columns `CLIMATE.REGION` and `PC.REALGSP.STATE`, since both will be accesible during the duration of the outage.

---
## BASELINE MODEL

Our baseline model for the prediction problem was a linear regression model that used three features: `CLIMATE.REGION`, `PC.REALGSP.STATE`, and `CAUSE.CATEGORY.DETAIL`. The columns `CLIMATE.REGION` and `CAUSE.CATEGORY.DETAIL` are nominal datatypes, therefore we transformed them using one hot encoding. The column `PC.REALGSP.STATE` contains quantitative data. We did not transform that column in any way. 

The performance of our baseline model was quite poor. When tested with training data, it received an RMSE of 3980.22 and when tested with unseen data, it received an RMSE of 7033.72. These results do not translate to a reliable predictive model. The poor performance of this model is due to the fact that we haven't found a sufficient combination of features to predict `OUTAGE.DURATION` yet.

---
## Final Model

For our final model we decided to change our model from a linear regression to a Random Forest Regressor in order to make certain modifcations in attempts to decrease our root mean sqaured error. We realized that the columns we used in our baseline model are not fully correlated to the `OUTAGE.DURATION` so we first found all the columns in the dataset that are correlated to our target column. Additionally, we automated the feature evaluating process, which found the features that produced the least root mean sqaured errors. This helped us find the features to include in our model and also realize that features we used in the baseline model were hurting our model's accuracy.

Our Final Model included the features 
















----
## Fairness Analysis
For our fairness analysis we decided to look at the groups of outage duration before 2011 and after 2011 to see if our model is fair. Our group X will be outage duration before 2011 and our group Y wil be the outage duration after 2011. The evalution metric we will be using is **Root Mean Squared Error(RMSE)** with a **signficance value of 0.05**. We will be using a permutation test in order to figure out the fairness of our model and our observed test statistic will be the **absolute difference in RMSE**.

*PERMUTATION TEST*

***Null Hypothesis*** : Our model is fair, precison for outage duration before 2011 and after 2011 are roughly the same and any differences are due to random chance

***Alternative Hypothesis*** : Our model is unfair, precison for outage duration before 2011 is lower than the outage duration after 2011 

*CONCLUSION*
First we found an observed RMSE difference of 723.42 and then conducted a permutation test of 1000 simulations. As a result, we got a p-value of 0.722 indicating that we reject the null hypothesis and concluding that the precison for outage duration before 2011 and after 2011 are roughly the same, concluding that our model is fair.

<iframe
  src="assets/emp-dist-4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
