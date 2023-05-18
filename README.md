
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

The key word here is "unusually." Sure, an outage may be long from time to time, but when is that length *statistically significant* or *unusual*? We find that addressing such a question has room for comparison to multiple variables and drawing unique parallels. Eventually, we might be able to reason or speculate as to *why* our offender states end up having these longer outages.

¹ Link to data source found [here.](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks)

² Descriptions of data columns courtesy of persons involved in [this paper.](https://www.sciencedirect.com/science/article/pii/S2352340918307182)

---

But just because every outage is different, it doesn't mean we can't find common threads or patterns that tell us ==valuable information== about such a trend.


Thanks to **Purdue University's LASCI (Laboratory For Advancing Sustainable Critical Infrastructure)**,[^1] we have access to ==1534 rows== of power outage data that contain valuable column information[^2] like:

<br />
<br />

- **OUTAGE.DURATION**
>"Duration of outage event (in minutes)." This is one of the most stark characteristics of a power outage.


- **POSTAL.CODE**
>"Represents the postal code of the U.S. states." Useful as an abbreviation or indicator for individual states.

- **CUSTOMERS.AFFECTED**
>"Number of customers affected by the power outage event." Gives perspective on size or magnitude of the outage.

\
\
\
With these data in mind, we believe an interesting question that naturally arises is:

> #### Every state has power outages from time to time, but which states' outages appear to be unusually long?

The key word here is "unusually." Sure, an outage may be long from time to time, but when is that length *statistically significant* or *unusual*? We find that addressing such a question has room for comparison to multiple variables and drawing unique parallels. Eventually, we might be able to reason or speculate as to *why* our offender states end up having these longer outages.

[1^] Link to data source found [here.](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks)

[2^] Descriptions of data columns courtesy of persons involved in [this paper.](https://www.sciencedirect.com/science/article/pii/S2352340918307182)


## CLEANING AND EDA 🔋

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

---
### UNIVARIATE ANALYSIS

We used plotly.express.choropleth to get an idea of the **geographical distribution of our OUTAGE.DURATION data.** We mapped out the mean durations of power outages onto their respective states and noticed that **higher means were typically distributed among more populated/urbanized areas.**

[CHORODURATION]

Perhaps there was a causal explanation for the distribution here, but we couldn't say for certain. Regardless, it gave us an inkling that perhaps we should see **how CUSTOMERS.AFFECTED was spread geographically and see if there was similarity**, since more densely-populated areas tend to have more people that necessitate electricity as a utility.

[CHOROCUSTOMERS]

And sure enough, it seemed as though similar regions (such as New York or California) were decently dense with dissatisfied customers. Now it was time to compare them against each other.

---
### BIVARIATE ANALYSIS

To start, we took the raw data from CUSTOMERS.AFFECTED and plotted it against OUTAGE.DURATION as a scatterplot.

[INDIVIDUALSCATTER]

Oh... that doesn't look that good.

Not only was the relationship relatively non-linear, but there were clearly outliers mixed up in the noise of **~1500 outages**. To our dismay, we noticed a slightly decreasing trend instead of an increasing one, which was expected. 

However, when we stratified our data by state and **plotted the means per state instead, we reached a strange conclusion.**

[MEANSCATTER]

Now there was a *much* more clear positive linear trend instead of a negative one from our previous plot. Why was this the case? We can chalk a lot of it up to **Simpson's Paradox: where data on the individual level tells a different story on a grouped level.**

On the individual level, outages that affected more and more people take less time to correct; speculating, this may have something to do with more widespread outages getting greater **sense of urgency and attention** in their correction.

On the state level, outages that affected more and more people take much longer to correct; this could be due to the fact that on the whole more populous states have more **complex power grids and systems** that take professionals longer to diagnose the problem due to difficulty.

<br />

---
### INTERESTING AGGREGATES

We started thinking more about the causes behind these outages and *why* they were occuring. So, we crafted a pivot table with the each of the 50 states as rows and each of the six listed causes (conveniently found in a column titled CAUSE.CATEGORY) as columns. The values of the boxes inside would be the total counts of how many of which causes sparked outages in which states.

It's pretty massive, but here's the table:

[PIVOT]

One interesting thing you'll notice is that "severe weather" is the most common cause of outages *by far.* It is the only column in this table that where **more than one state has a number of outages in the double digits.**

A second thing worth note is that there are a lot of zeroes. This indicates that **not all states frequently experience all causes for specific outages.** In other words, certain states are more predisposed to certain susceptibilities than others, which lines up with most peoples anecdotal knowledge of certain cities or states being worse for power outages.



## ASSESSMENT OF MISSINGNESS 🔋
c

## HYPOTHESIS TESTING 🔋
d