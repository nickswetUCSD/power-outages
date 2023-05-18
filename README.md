

## INTRODUCTION 🔋

Power outages in the United States are a prime example of a common occurance that has a lots of *slight* variation over a population. For example, it is commonly believed that power outages vary in intensity and duration due to:


- Temperature of locale
- Local power demand
- Humidity and presence of storms
- Natural disasters
- Even (sadly) animals



And of thanks to all of these variables, no two outages will ever quite be the same. 

---

But just because every outage is different, it doesn't mean we can't find common threads or patterns that tell us valuable information about such a trend.

Thanks to **Purdue University's LASCI (Laboratory For Advancing Sustainable Critical Infrastructure)**,¹ we have access to **1534 rows** of power outage data that contain valuable column information² like:

<br />

- **OUTAGE.DURATION**:
>"Duration of outage event (in minutes)." This is one of the most stark characteristics of a power outage.
<br />

- **POSTAL.CODE**
>"Represents the postal code of the U.S. states." Useful as an abbreviation or indicator for individual states.
<br />

- **CUSTOMERS.AFFECTED**
>"Number of customers affected by the power outage event." Gives perspective on size or magnitude of the outage.

<br />
<br />
<br />
With these data in mind, we believe an interesting question that naturally arises is:

<br />
<br />

> #### Every state has power outages from time to time, but which states' outages appear to be unusually long?

<br />
<br />

The key word here is "unusually." Sure, an outage may be long from time to time, but when is that length *statistically significant* or *unusual*? We find that addressing such a question has room for comparison to multiple variables and drawing unique parallels. Eventually, we might be able to reason or speculate as to *why* our offender states end up having these longer outages.

¹ Link to data source found [here.](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks)

² Descriptions of data columns courtesy of persons involved in [this paper.](https://www.sciencedirect.com/science/article/pii/S2352340918307182)

---
## CLEANING AND E.D.A. (EXPLORATORY DATA ANALYSIS)🔋

We started our cleaning off by converting the default excel data file to a CSV file so that we could access and visualize the data through Python Pandas, and after some slight reformatting we had a properly-indexed DataFrame to work with.

[CLEAN1]

We proceeded by creating two new columns through merging old unnecessary ones:
<br />

- **OUTAGE.START**:
>Was the column merging of "OUTAGE.START.DATE" and "OUTAGE.START.TIME" into a single pd.Timestamp column.

<br />

- **OUTAGE.RESTORATION:**
>Was the column merging of "OUTAGE.RESTORATION.DATE" and "OUTAGE.RESTORATION.TIME" into a single pd.Timestamp column.

[CLEAN2]

These steps made our analysis a lot easier to work with, and a lot more "date-centric". We could focus more on OUTAGE.DURATION and the start and endpoints of our outages instead of getting confused among five different columns relating to time, so things naturally became more efficient. The resulting dataframe looked something like this.

[CLEANDF]

<br />

---
### UNIVARIATE ANALYSIS

We used plotly.express.choropleth to get an idea of the **geographical distribution of our OUTAGE.DURATION data.** We mapped out the mean durations of power outages onto their respective states and noticed that **higher means were typically distributed among more populated/urbanized areas.**

[CHORODURATION]

Perhaps there was a causal explanation for the distribution here, but we couldn't say for certain. Regardless, it gave us an inkling that perhaps we should see **how CUSTOMERS.AFFECTED was spread geographically and see if there was similarity**, since more densely-populated areas tend to have more people that necessitate electricity as a utility.

[CHOROCUSTOMERS]

And sure enough, it seemed as though similar regions (such as New York or California) were decently dense with dissatisfied customers. Now it was time to compare them against each other.

<br />

---
### BIVARIATE ANALYSIS

To start, we took the raw data from CUSTOMERS.AFFECTED and plotted it against OUTAGE.DURATION as a scatterplot.

[INDIVIDUALSCATTER]

Oh... that doesn't look that good.

Not only was the relationship relatively non-linear, but there were clearly outliers mixed up in the noise of **~1500 outages**. To our dismay, we noticed a slightly decreasing trend instead of an increasing one, which was expected. 

However, when we stratified our data by state and **plotted the means per state instead, we reached a strange conclusion.**

[MEANSCATTER]

Now there was a *much* more clear positive linear trend. Why do these two plots show opposite conclusions? We can chalk a lot of it up to **Simpson's Paradox: where data on the individual level tells a different story on a grouped level.**

On the individual level, outages that affect more and more people take less time to correct; speculating, this may have something to do with more widespread outages getting greater **sense of urgency and attention** in their correction.

On the state level, outages that affect more and more people take much longer to correct; this could be due to the fact that on the whole more populous states have more **complex power grids and systems** that take professionals significantly longer to diagnose the problem.

<br />

---
### INTERESTING AGGREGATES

We started thinking more about the causes behind these outages and *why* they were occuring. So, we crafted a pivot table with the each of the 50 states as rows and each of the six listed causes (conveniently found in a column titled CAUSE.CATEGORY) as columns. The values of the boxes inside would be the total counts of how many of which causes sparked outages in which states.

It's pretty massive, but here's the table:

[PIVOT]

One interesting thing you'll notice is that "severe weather" is the most common cause of outages *by far.* It is the only column in this table that where **more than one state has a number of outages in the double digits.**

A second thing worth note is that there are a lot of zeroes. This indicates that **not all states frequently experience all causes for specific outages.** In other words, certain states are more predisposed to certain susceptibilities than others, which lines up with most peoples' anecdotal knowledge of certain cities or states being worse for power outages.

---


## ASSESSMENT OF MISSINGNESS 🔋

Data can be absent from a dataset for many reasons. Sometimes it's intentional, other times it's not. Typically, it is good to assess the type of missingness in a dataset before taking your next steps.

### NMAR ANALYSIS

NMAR (or not Not Missing At Random) is a type of missingness characterized by data in a column being missing because of the value of the missing data itself. 

For example: If you get a bunch of missing values after administering a self-reported survey with the sole question "Rate your happiness on a scale of 1 to 10", it's probably the twos and threes that are missing from your dataset, because less happy individuals are more inclined to be embarassed to report anything at all. Since the data that's missing is dependent on if that number is low, the missingness in the data here is NMAR.

In our power outage data, we could consider missing values in the CAUSE.CATEGORY.DETAIL column as NMAR. The purpose of CAUSE.CATEGORY.DETAIL is to supply extra information about the cause of an outage if necessary. It's likely the case that information was left missing from the column if that extra information was not considered relevant or necessary. Though we cannot know for sure if that is the exact criteria for whether or not missing values were left unattended, the fact that such a practice is likely tells us that this data is NMAR. 

The missingness of the CAUSE.CATEGORY.DETAIL column is dependent on whether or not the extra description is relevant and necessary. 

<br/>

---
### MISSINGNESS DEPENDENCY

Our last comparison of CUSTOMERS.AFFECTED and OUTAGE.DURATION was only made possible by dropping rows with missing values from the dataset, but how do we know that dropping those values didn't significantly impact our conclusion? Moreover, if we were to fill in (or impute) these missing values, how would we know *that* wouldn't significantly impact our conclusion?

**We need to assess whether or not data in our CUSTOMERS.AFFECTED column is MAR (Missing at Random) or MCAR (Missing Completely At Random) with respect to OUTAGE.DURATION.**

- If our data winds up MCAR, we can be more liberal with how we tackle the data, because "Missing Completely At Random" means tweaking data at random **won't significantly change our distribution of OUTAGE.DURATION data.**

- If our data winds up MAR, we have to be more careful with how we tackle the data, because "Missing At Random" means tweaking data at random ***might* significantly change our distribution of OUTAGE.DURATION data.**

Here's a diagram of two distributions, one of OUTAGE.DURATION but only when CUSTOMERS.AFFECTED **has missing data**, and another of OUTAGE.DURATION but only when CUSTOMERS.AFFECTED **has present data**:

[DIST1]
[DIST2]

<br>
All things considered, the distributions have roughly the same shape; both of them are **right-skewed and *lot* of data is packed into that first bar.** This implies that our OUTAGE.DURATION data is of the same distribution regardless of whether or not CUSTOMERS.AFFECTED is absent, meaning **our data is probably MCAR.** But looks can be deceiving, so we more formally decided to run a permutation test and see what results lie waiting.

<br>
[PERMTEST]

Surprisingly, the p-value we recieved from running this test was ~0.02, which (at a standard α = 0.05 significance level) **implies significant difference in the missing and non-missing distributions, or MAR dependence.**

It's a good thing we checked! It turns out we need to be more careful with our data.



## HYPOTHESIS TESTING 🔋

In our hypothesis test, we investigated whether each of the fifty states had longer power outages (duration) compared to the average power outage duration of the greater United States. We defined long power outages as power outages that were greater than the national average duration. Thus, in order to simulate our null hypothesis, we created a boolean column of True’s for longer than the average duration and False for not longer than the average duration for each observation. Thus, our hypotheses were as follows:
Null Hypothesis: The state’s proportion of long power outages is equal to proportion of long power outages of the greater United States
Alternative Hypothesis: the state’s proportion of of long power outages is greater than the proportion of long power outages of the greater United States
Test Statistic: the proportion of long power outages 
Significance Level: 0.05

These choices of hypothesis fit our overall question wherein we are trying to determine which states have more longer than average power outages. With this test, we hope to perhaps find regional patterns of where long power outages may occur. 
 
P-Value: Since we conducted the hypothesis test for each state, we have 50 p-values: one for each state indicating the probability of seeing such an observed proportion of long power outages if the null hypothesis was indeed true. We have stored our p-values in the following DataFrame:


Looking at each p-value, we can fail to reject the null hypothesis for a majority of the states (46 total states) as they all have p-values greater than 0.05. However, for 4 states, we can reject the null hypothesis at the 5% significance level and conclude that Pennsylvania, Michigan, New York, and New Jersey do not have a proportion of long power outages equal to the greater United States. It’s interesting to note that all of these states are located in the Northeast and Midwest (for Michigan), perhaps suggesting there are regional factors at play impacting power outage duration. Moreover, it is also interesting to note that Wisconsin did not appear on this list despite having the greatest mean power outage duration overall. 
However, it is important to note that with such multiple testing without corrections, there is an increased risk of committing a Type 1 error wherein we reject the null hypothesis when it is actually true. 

