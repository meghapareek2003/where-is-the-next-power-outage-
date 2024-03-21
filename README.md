# Where is the Next Power Outage?

## Introduction
Welcome to our power outage duration prediction project, where we aim to develop a robust machine learning model capable of accurately forecasting the duration of power outages. This project addresses a critical need in understanding and mitigating the impact of power disruptions by leveraging various features such as climate conditions, outage causes, and temporal factors.

Our focus is on predicting the duration of power outages, which is a vital metric for emergency response planning and infrastructure resilience. By accurately forecasting outage durations, our model can assist utility companies, emergency responders, and policymakers in allocating resources effectively and implementing timely interventions during outages.

The dataset used for this analysis contains valuable information on power outages across different regions of the United States, offering insights into outage patterns, contributing factors, and their consequences. By exploring this dataset, we aim to uncover hidden patterns and relationships that can enhance our understanding of outage dynamics

Our dataset has 1534 rows of data and contains major power outage data in the continental United States from January 2000 to July 2016.

Some of the relavant columns are: 
CLIMATE.REGION: Represents the climate region where the outage occurred. This categorical variable provides information about the geographical location and climate conditions of the outage area.

ANOMALY.LEVEL: Indicates the anomaly level associated with the outage. This numerical variable quantifies the deviation from normal operating conditions and serves as a proxy for the severity of the outage.

CAUSE.CATEGORY: Describes the category of the outage cause. This categorical variable provides insights into the primary reasons behind power disruptions, such as severe weather, equipment failure, or human intervention.

OUTAGE.DURATION: Represents the duration of the power outage in minutes. This is the target variable that we aim to predict using machine learning models based on the other features in the dataset.

U.S._STATE: Indicates the U.S. state where the outage occurred. This categorical variable provides information about the geographic location of the outage, which may influence outage response and recovery efforts.

## Data Cleaning and Exploratory Data Analysis
The data cleaning process aimed to ensure the integrity and usability of the dataset for subsequent analysis. The initial steps involved adjusting the DataFrame's structure, such as setting the appropriate column names and removing extraneous rows. This ensured that the data aligns correctly with its intended format and layout.

Following this, date and time information, crucial for understanding outage durations and restoration times, was consolidated into single datetime columns. This consolidation facilitates easier analysis of outage events over time and enables the calculation of outage durations.

Addressing missing or invalid values is vital to prevent skewing analysis results. The process involved replacing various representations of missing data, such as 'NaN', 'N/A', or 'na', with a consistent representation, `pd.NA`, signifying missing values.

Ensuring correct data types is essential for accurate analysis. Consequently, columns representing numerical values, such as observation counts, years, and months, were converted to the appropriate integer data type. Additionally, the 'MONTH' column, which occasionally contained missing values, was first filled with zeros before conversion to integers to maintain data consistency.

Finally, columns containing date and time information were converted to datetime objects, facilitating easier manipulation and analysis of temporal data.

Overall, these data cleaning steps aimed to prepare the dataset for subsequent exploratory data analysis and modeling. The cleaned DataFrame now exhibits a standardized structure, consistent data types, and appropriately handled missing values, providing a solid foundation for further analysis.

| YEAR | MONTH | U.S._STATE | CLIMATE.REGION | ANOMALY.LEVEL | CLIMATE.CATEGORY | OUTAGE.DURATION | CUSTOMERS.AFFECTED |
|------|-------|------------|----------------|----------------|-------------------|------------------|--------------------|
| 2011 | 7     | Minnesota  | East North Central | -0.3         | normal            | 3060.0           | 70000.0            |
| 2014 | 5     | Minnesota  | East North Central | -0.1         | normal            | 1.0              | NaN                |
| 2010 | 10    | Minnesota  | East North Central | -1.5         | cold              | 3000.0           | 70000.0            |
| 2012 | 6     | Minnesota  | East North Central | -0.1         | normal            | 2550.0           | 68200.0            |
| 2015 | 7     | Minnesota  | East North Central | 1.2          | warm              | 1740.0           | 250000.0           |


Univariate Analysis:

<iframe
  src="assests/climate_category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The bar chart illustrates the distribution of climate categories and their potential impact on power outages. 'Normal' emerges as the most prevalent category, indicating that extreme climate conditions may not significantly influence outage frequency. However, it's noteworthy that 'cold' exhibits a slightly higher occurrence compared to 'warm', suggesting a nuanced relationship between climate and outage vulnerability.  This is something we can consider in our model.

<iframe
  src="assests/causes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To further see if climate can be one of the causes, we created a bar plot to see the distribution of the cause of power outages. As seen, 'severe weather' is a prominent cause. 

Bivariate Analysis
<iframe
  src="assests/mean_outage_by_month.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
On plotting the mean outage duration by month we realized that August seems to have the highest outage durations. One intresting observation is that the duration is somewhat cyclical.
<iframe
  src="assests/customers_by_year.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
On plotting customers affected per year, we can see how somewhat more customers were affected before the year 2010.

## Assessment of Missingness

We would say that the column DEMAND.LOSS.MW is NMAR. This column shows peak demand lost during an outage. It is NMAR because when we encounter a power outage, it is a hassle and challenging to keep track of the demand that was lost throughout the duration of the outage. It is to be noted that power outages as well as restorations can be an uncertain process. As such, we can’t fully predict and it would make it really difficult to record demand that we lost in a consistent manner. It also makes sense that some of the DEMAND.LOSS.MW column values are missing because if we were to assess an area’s capability to persistently keep track of lost data, we would observe that certain areas are not able to record and report lost demand data.

To asses the Missingness Dependency of the CUSTOMERS.AFFECTED column, we conducted 2 permutation tests.

In the first test, we want to determine by analyzing if the missingness of the CUSTOMERS.AFFECTED column is dependent on the CAUSE.CATEGORY column.

<iframe
  src="assests/missingness1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We computed the p_value as 0.0 and that is less than our significance level. As such, we reject the null hypothesis where we stated that the distribution of "CAUSE.CATEGORY" when "CUSTOMERS.AFFECTED" is missing is the same as the distribution of "CAUSE.CATEGORY" when "CUSTOMERS.AFFECTED" isn't missing. Our conclusion as a result is that the missingness in the "CUSTOMERS.AFFECTED" column is *dependent* on "CAUSE.CATEGORY".

In the second test, we wanted to examine if missingness of the "CUSTOMERS.AFFECTED" column is dependent on the "CLIMATE.CATEGORY" column.
<iframe
  src="assests/missingness2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We computed the p value of approximately 0.346 which does fluctuate each time we run. However the p value is more than our significance level. As a result, we fail to reject the null hypothesis where we stated that the distribution of "CLIMATE.CATEGORY" when "CUSTOMERS.AFFECTED" is missing is the same as the distribution of "CLIMATE.CATEGORY" when "CUSTOMERS.AFFECTED" is not missing. We conclude that the missingness in the "CUSTOMERS.AFFECTED" column is NOT dependent on "CLIMATE.CATEGORY".


## Hypothesis Testing
**Question:** Are American states located in the midwest and east more likely to experience significant power outages, lasting 1000 minutes or longer, on cold days?

**Null Hypothesis:** The chances of severe power outage arising in the East North central region on cold days is the same as any other region on other days.

**Alternative Hypothesis:** There are chances of experiencing severe power outages more frequently in the East North central region on cold days compared to other regions on other days.

The test statistic is the proportion of severe power outages, with our significant level being 0.05. We obtained a p value of 0.00016, which is less than the significance level of 5%. As such, we would reject the null hypothesis. 
Explanation: Based on the above, we would say that our test statistics and hypothesis effectively meet the standards to respond to the question, where test statistics represent how often we see power outages arising. Under the null hypothesis, we were then able to analyze this from our dataset. Moreover, we were also able to analyze the region in the East North Central on cold days and execute the simulations. Given the large number of simulations, we again noticed that the p-value was less than the significance level. In response to our simulations, we conclude that the occurrence of power outages in the East North Central region was more frequent on cold days and was vital, as well as most likely not caused by chance. 


## Framing a Prediction Problem
[Description of how the problem of predicting power outages was formulated.]

## Baseline Model

In our base model, we utilized a Random Forest Classifier as the primary algorithm for predicting power outage durations. This choice was made due to the classifier's ability to handle both numerical and categorical features effectively, as well as its capability to capture complex relationships within the data.

In our model, we have a total of nine features, including:

Quantitative Features:

'YEAR': Year of the outage event.
'MONTH': Month of the outage event.
'OUTAGE.DURATION': Duration of the outage in hours.
'ANOMALY.LEVEL': Level of anomaly associated with the outage event.
Nominal Features (Categorical):

'CLIMATE.REGION': Climate region where the outage occurred.
'CLIMATE.CATEGORY': Climate category associated with the outage event.
'U.S._STATE': State where the outage occurred.
'CAUSE.CATEGORY': Category of the cause of the outage.

These features are utilized to train our model and predict power outage durations based on various factors such as climate conditions, location, outage causes, and anomaly levels.


To handle the categorical features in our model, we performed label encoding using scikit-learn's LabelEncoder. This encoding technique assigns a unique numerical label to each category within a categorical feature. 

The performance of our current model, based on the accuracy score obtained using pl_base.score(X_test_base, y_test_base), is approximately 0.09126. This indicates that our model correctly predicts the outage duration around 9.13% of the time on the test set. However, a score of 0.09126 suggests that our model's performance is quite poor. In other words, our current model is not effective in accurately predicting power outage durations based on the selected features.This current baseline model is not good based off this performance.

## Final Model
[Description of the final predictive model, its features, and evaluation metrics.]

## Fairness Analysis
