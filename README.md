# BERLEN'S A BOT
Project for DSC80 at UCSD

By Berlen Zhang and Sohan Raval

## INTRODUCTION
This project aims to analyze what factors have the greatest impact on the severity of power outages. In this body of work, we answered the question: do different regions and causes affect the severity of power outages in the US? Additionally, we developed a predictive model that forecasts the duration of power outages given a set of details of the outage. This will be a useful tool that allows utility companies to enhance communication with customers when an outage occurs. Being able to inform the public about how long an outage is predicted to last will help in managing and mitigating the consequences of power outages.


The dataset we used to generate an answer to our project question contains details of 1535 power outages in the US, each outage being represented by a row. It includes 57 columns containing specific information on the outage. We only used 24 of these columns in our project.


Below is a description of all the columns we will be working with:

- `YEAR`: The year a power outage took place.

- `MONTH` : The month a power outage took place.

- `U.S._STATE`: The state where the power outage took place.

- `POSTAL.CODE`:The abbreviation of the state column.

- `NERC.REGION`: The region in which the North American Electric Reliability Corporation was present in response to the outage.

- `CLIMATE.REGION`: 9 regions consistent with the climate as produced by the National Centers for Environmental Information(NCEI) scientists.

- `ANOMALY.LEVEL`: 

- `CLIMATE.CATEGORY`: The state of the Climate when the outage occurred.

- `CAUSE.CATEGORY`: The cause of the outage.

- `CAUSE.CATEGORY.DETAIL`: More in-depth information about the cause of the outage.

- `OUTAGE.DURATION`: How long the outage lasted in minutes

- `DEMAND.LOSS.MW`: 

- `CUSTOMERS.AFFECTED`: The amount of customers affected by the outage

- `TOTAL.PRICE`:

- `TOTAL.SALES`:

- `PC.REALGSP.STATE`: The economic output of a state at the time of the outage.

- `PC.REALGSP.USA`: The economic output of the US at the time of the outage.

- `TOTAL.REALGSP`: 

- `POPULATION`(int): Population of the area affected by the outage

- `POPDEN_URBAN`: The population density of urban areas in a specified state.

- `POPDEN_RURAL`:  The population density of rural areas in a specified state.

- `OUTAGE.START`: A timestamp of when the outage started

- `OUTAGE.RESTORATION`: A timestamp of when the outage ended

---
## DATA CLEANING AND EXPLORATORY ANALYSIS

**Data Cleaning**
We cleaned the data to better fit our objectives with the following steps:
1. Edit data frame to only contain the columns we are interested in utilizing in our project:
  * We dropped columns that would be unnecessary for predicting the duration of a power outage.        The 24 columns displayed above are the ones we decided would be useful for the project.
2. Replace the zero values in columns `CUSTOMERS.AFFECTED` and `OUTAGE.DURATION` with nan:
  * It would be unrealistic for an outage to have a duration of zero since this would mean there       was no outage at all. It is also impossible for there to be zero customers affected since this     would signify that no one was there to witness or report the power outage.
3. Converting columns `OUTAGE.DURATION` and `DEMAND.LOSS.MW` into suitable data types:
  * The data in the two columns were originally string values. In order to use them in the             necessary data analysis procedures we would conduct, we had to convert them into float values.
4. Combining columns `OUTAGE.START.DATE` and  `OUTAGE.START.TIME` into a column named                 `OUTAGE.STARAT`:
  * Originally these columns contained string type values. We combined them into a column and          converted them into timestamp objects, making the data more convenient to use in calculations.
5. Combining columns `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into a column named      `OUTAGE.RESTORATION`:
  * Originally these columns contained string type values. We combined them into a column and          converted them into timestamp objects, making the data more convenient to use in calculations.

---
## ASSESSMENT OF MISSNGNESS

In this part of our analysis we look at the missing values in our dataset for columns `RESTORATION.TIME` and `DEMAND.LOSS.MW`


### NMAR ANALYSIS

We believe that the `RESTORATION.TIME` column is likely NMAR. If the restoration time of an outage is late at night, there is a greater chance of it being missing since the services that keep track of outage information may be unavailable at that time. Additional data we would obtain to explain the missingness of `RESTORATION.TIME` would be the city in which the outage occurred. We believe certain cities may have less reliable data recording services. Additionally, in order to find out if `RESTORATION.TIME` is Missing at Random(MAR), we can possibly look at which companies are responsible for the power outages in that particular city to check if their recording services are reliable, consequently determining if the missingness is dependent on the company’s recording services.

### MISSINGNESS DEPENDENCY

To figure out Missingness dependency we will conduct permutation tests on the `DEMAND.LOSS.MW` column based on the columns `CAUSE.CATEGORY` and `CLIMATE.CATEGORY`. In both permutation tests we used the Total Variation Distance(TVD) test statistic and used a 0.5 significance level.

*PERMUTATION TEST FOR CAUSE.CATEGORY:*

***Null Hypothesis*** : The distribution of the data in the column `CAUSE.CATEGORY` column is the same regardless of the missingness of the column `CAUSE.CATEGORY.DETAIL`. 

***Alternative Hypothesis*** : The distribution of data in the column `CAUSE.CATEGORY` is different depending on the missingess of the column `DEMAND.LOSS.MW`.

***Results*** :After conducting the permutation test, we found a TVD observed test statistic of 0.179 and a p-value of 0.0, as a result, we reject the null hypothesis. The graph below shows the empirical distribution of this test which indicates that the `DEMAND.LOSS.MW` missigness is dependent on the `CAUSE.CATEGORY` column. The **red line** on the graph represents the observed test statistic.

<iframe
  src="assets/emp-dist-1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

*PERMUTATION TEST FOR CLIMATE.CATEGORY:*

***Null Hypothesis*** : The distribution of the data in the column `CLIMATE.CATEGORY` is the same regardless of the missingess of the column `DEMAND.LOSS.MW`.

***Alternative Hypothesis*** : The distribution of the data in the column `CLIMATE.CATEGORY` is different depending on the missingness of the column `DEMAND.LOSS.MW`.

***Results*** :For the second permutation test, we found a TVD observed test statistic of 0.034 and a p-value of 0.314, therefore, we fail to reject the null hypothesis which indicates that the missing values of the 'DEMAND.LOSS.MW column does not depend on the `CLIMATE.CATEGORY` column. The graph below is the empirical distribution of this test and the **red line** on the graph represents the observed test statstic.

<iframe
  src="assets/emp-dist-2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
## HYPOTHESIS TESTING







