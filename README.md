# **Title**
Project for DSC80 at UCSD

By Berlen Zhang and Sohan Raval

## **Introduction**
This project aims to analyze what factors have the greatest impact on the severity of power outages. In this body of work, we answered the question: do different regions and causes affect the severity of power outages in the US? Additionally, we developed a predictive model that forecasts the duration of power outages given a set of details of the outage. This will be a useful tool that allows utility companies to enhance communication with customers when an outage occurs. Being able to inform the public about how long an outage is predicted to last will help in managing and mitigating the consequences of power outages.


The dataset we used to generate an answer to our project question contains details of 1535 power outages in the US, each outage being represented by a row. It includes 57 columns containing specific information on the outage. We only used 24 of these columns in our project.


Below is a description of all the columns we will be working with:
1.`YEAR`: The year a power outage took place.

2.`MONTH` : The month a power outage took place.

3.`U.S._STATE`: The state where the power outage took place.

4.`POSTAL.CODE`:The abbreviation of the state column.

5.`NERC.REGION`: The region in which the North American Electric Reliability Corporation was present in response to the outage.

6.`CLIMATE.REGION`: 9 regions consistent with the climate as produced by the National Centers for Environmental Information(NCEI) scientists.

7.`ANOMALY.LEVEL`: 

8.`CLIMATE.CATEGORY`: The state of the Climate when the outage occurred.

9.`CAUSE.CATEGORY`: The cause of the outage.

10.`CAUSE.CATEGORY.DETAIL`: More in-depth information about the cause of the outage.

11.`OUTAGE.DURATION`: How long the outage lasted in minutes

12.`DEMAND.LOSS.MW`: 

13.`CUSTOMERS.AFFECTED`(int): The amount of customers affected by the outage

14.`TOTAL.PRICE`:

15.`TOTAL.SALES`:

16.`PC.REALGSP.STATE`: The economic output of a state at the time of the outage.

17`PC.REALGSP.USA`: The economic output of the US at the time of the outage.

18.`TOTAL.REALGSP`: 

19.`POPULATION`(int): Population of the area affected by the outage

20.`POPDEN_URBAN`: The population density of urban areas in a specified state.

21.`POPDEN_RURAL`:  The population density of rural areas in a specified state.

22.`OUTAGE.START`: A timestamp of when the outage started

23.`OUTAGE.RESTORATION`: A timestamp of when the outage ended


