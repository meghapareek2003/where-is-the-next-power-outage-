# Where is the Next Power Outage?

## Introduction


## Data Cleaning and Exploratory Data Analysis
The data cleaning process aimed to ensure the integrity and usability of the dataset for subsequent analysis. The initial steps involved adjusting the DataFrame's structure, such as setting the appropriate column names and removing extraneous rows. This ensured that the data aligns correctly with its intended format and layout.

Following this, date and time information, crucial for understanding outage durations and restoration times, was consolidated into single datetime columns. This consolidation facilitates easier analysis of outage events over time and enables the calculation of outage durations.

Addressing missing or invalid values is vital to prevent skewing analysis results. The process involved replacing various representations of missing data, such as 'NaN', 'N/A', or 'na', with a consistent representation, `pd.NA`, signifying missing values.

Ensuring correct data types is essential for accurate analysis. Consequently, columns representing numerical values, such as observation counts, years, and months, were converted to the appropriate integer data type. Additionally, the 'MONTH' column, which occasionally contained missing values, was first filled with zeros before conversion to integers to maintain data consistency.

Finally, columns containing date and time information were converted to datetime objects, facilitating easier manipulation and analysis of temporal data.

Overall, these data cleaning steps aimed to prepare the dataset for subsequent exploratory data analysis and modeling. The cleaned DataFrame now exhibits a standardized structure, consistent data types, and appropriately handled missing values, providing a solid foundation for further analysis.

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



## Hypothesis Testing
[Explanation of any hypotheses tested during the analysis and their results.]

## Framing a Prediction Problem
[Description of how the problem of predicting power outages was formulated.]

## Baseline Model
[Details of the baseline model used for prediction and its performance metrics.]

## Final Model
[Description of the final predictive model, its features, and evaluation metrics.]

## Fairness Analysis
