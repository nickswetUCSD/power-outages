
## INTRODUCTION 🔋

Power outages in the United States are a prime example of a common occurance that has a lots of *slight* variation over a population. For example, it is commonly believed that power outages vary in intensity and duration due to:


- Temperature of locale
- Local power demand
- Humidity and presence of storms
- Natural disasters
- Even (sadly) animal interference



And thanks to all of these variables, no two outages will ever quite be the same. 

---

But just because every outage is different, it doesn't mean we can't find common threads or patterns that tell us valuable information about such a trend. Such information is important for people to know. People in states who may be more prone to more frequent or longer power outages should be aware of the fact and prepare accordingly by stocking food/water, having a generator, ... etc.

Thanks to **Purdue University's LASCI (Laboratory For Advancing Sustainable Critical Infrastructure)**,¹ we have access to **1534 rows** of power outage data that contain valuable column information² like:

<br />

- **OUTAGE.DURATION** ⏱:
>"Duration of outage event (in minutes)." This is one of the most stark characteristics of a power outage.
<br />

- **CAUSE.CATEGORY.DETAIL**🌪️:
>"Detailed description of the event categories causing the major power outages." Interesting metric to track and aggregate.
<br />

- **CUSTOMERS.AFFECTED**🚶:
>"Number of customers affected by the power outage event." Gives perspective on size or magnitude of the outage.

<br />
With these data in mind, we believe an interesting question that naturally arises is:


> ## Every state has power outages from time to time, but which states' outages appear to be unusually long?

<br />
<br />

The key word here is "unusually." Sure, an outage may be long from time to time, but when is that length *statistically significant* or *unusual*? We find that addressing such a question has room for comparison to multiple variables and drawing unique parallels. Eventually, we might be able to reason or speculate as to *why* our offender states end up having these longer outages.

¹ Link to data source found [here.](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks)

² Descriptions of data columns courtesy of persons involved in [this paper.](https://www.sciencedirect.com/science/article/pii/S2352340918307182)

---
## CLEANING AND E.D.A. (EXPLORATORY DATA ANALYSIS)🔋

We started our cleaning off by converting the default excel data file to a CSV file so that we could access and visualize the data through Python Pandas, and after some slight reformatting we had a properly-indexed DataFrame to work with.

<iframe src="assets/clean1.html" width=800 height=150 frameBorder=0></iframe>

We proceeded by creating two new columns through merging old unnecessary ones:
<br />

- **OUTAGE.START**:
>Was the column merging of "OUTAGE.START.DATE" and "OUTAGE.START.TIME" into a single pd.Timestamp column.

- **OUTAGE.RESTORATION:**
>Was the column merging of "OUTAGE.RESTORATION.DATE" and "OUTAGE.RESTORATION.TIME" into a single pd.Timestamp column.

These steps made our analysis a lot easier to work with, and a lot more "date-centric". We could focus more on OUTAGE.DURATION⏱ and the start and endpoints of our outages instead of getting confused among five different columns relating to time, so things naturally became more efficient. The resulting dataframe looked something like this.

<div style="overflow-x:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YEAR</th>
      <th>MONTH</th>
      <th>U.S._STATE</th>
      <th>POSTAL.CODE</th>
      <th>NERC.REGION</th>
      <th>CLIMATE.REGION</th>
      <th>ANOMALY.LEVEL</th>
      <th>CLIMATE.CATEGORY</th>
      <th>OUTAGE.START.DATE</th>
      <th>OUTAGE.START.TIME</th>
      <th>OUTAGE.RESTORATION.DATE</th>
      <th>OUTAGE.RESTORATION.TIME</th>
      <th>CAUSE.CATEGORY</th>
      <th>CAUSE.CATEGORY.DETAIL</th>
      <th>HURRICANE.NAMES</th>
      <th>OUTAGE.DURATION</th>
      <th>DEMAND.LOSS.MW</th>
      <th>CUSTOMERS.AFFECTED</th>
      <th>RES.PRICE</th>
      <th>COM.PRICE</th>
      <th>IND.PRICE</th>
      <th>TOTAL.PRICE</th>
      <th>RES.SALES</th>
      <th>COM.SALES</th>
      <th>IND.SALES</th>
      <th>TOTAL.SALES</th>
      <th>RES.PERCEN</th>
      <th>COM.PERCEN</th>
      <th>IND.PERCEN</th>
      <th>RES.CUSTOMERS</th>
      <th>COM.CUSTOMERS</th>
      <th>IND.CUSTOMERS</th>
      <th>TOTAL.CUSTOMERS</th>
      <th>RES.CUST.PCT</th>
      <th>COM.CUST.PCT</th>
      <th>IND.CUST.PCT</th>
      <th>PC.REALGSP.STATE</th>
      <th>PC.REALGSP.USA</th>
      <th>PC.REALGSP.REL</th>
      <th>PC.REALGSP.CHANGE</th>
      <th>UTIL.REALGSP</th>
      <th>TOTAL.REALGSP</th>
      <th>UTIL.CONTRI</th>
      <th>PI.UTIL.OFUSA</th>
      <th>POPULATION</th>
      <th>POPPCT_URBAN</th>
      <th>POPPCT_UC</th>
      <th>POPDEN_URBAN</th>
      <th>POPDEN_UC</th>
      <th>POPDEN_RURAL</th>
      <th>AREAPCT_URBAN</th>
      <th>AREAPCT_UC</th>
      <th>PCT_LAND</th>
      <th>PCT_WATER_TOT</th>
      <th>PCT_WATER_INLAND</th>
      <th>OUTAGE.START</th>
      <th>OUTAGE.RESTORATION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2011</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.3</td>
      <td>normal</td>
      <td>Friday, July 01, 2011</td>
      <td>5:00:00 PM</td>
      <td>Sunday, July 03, 2011</td>
      <td>8:00:00 PM</td>
      <td>severe weather</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3060.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>11.6</td>
      <td>9.18</td>
      <td>6.81</td>
      <td>9.28</td>
      <td>2332915</td>
      <td>2114774</td>
      <td>2113291</td>
      <td>6562520</td>
      <td>35.54907261</td>
      <td>32.22502941</td>
      <td>32.20243138</td>
      <td>2308736</td>
      <td>276286</td>
      <td>10673</td>
      <td>2595696</td>
      <td>88.9448</td>
      <td>10.6440</td>
      <td>0.4112</td>
      <td>51268</td>
      <td>47586</td>
      <td>1.077375699</td>
      <td>1.6</td>
      <td>4802</td>
      <td>274182</td>
      <td>1.751391412</td>
      <td>2.2</td>
      <td>5348119</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>2011-07-01 17:00:00</td>
      <td>2011-07-03 20:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014</td>
      <td>5</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>Sunday, May 11, 2014</td>
      <td>6:38:00 PM</td>
      <td>Sunday, May 11, 2014</td>
      <td>6:39:00 PM</td>
      <td>intentional attack</td>
      <td>vandalism</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.12</td>
      <td>9.71</td>
      <td>6.49</td>
      <td>9.28</td>
      <td>1586986</td>
      <td>1807756</td>
      <td>1887927</td>
      <td>5284231</td>
      <td>30.03248722</td>
      <td>34.21038936</td>
      <td>35.72756376</td>
      <td>2345860</td>
      <td>284978</td>
      <td>9898</td>
      <td>2640737</td>
      <td>88.8335</td>
      <td>10.7916</td>
      <td>0.3748</td>
      <td>53499</td>
      <td>49091</td>
      <td>1.089792426</td>
      <td>1.9</td>
      <td>5226</td>
      <td>291955</td>
      <td>1.790001884</td>
      <td>2.2</td>
      <td>5457125</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>2014-05-11 18:38:00</td>
      <td>2014-05-11 18:39:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>10</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-1.5</td>
      <td>cold</td>
      <td>Tuesday, October 26, 2010</td>
      <td>8:00:00 PM</td>
      <td>Thursday, October 28, 2010</td>
      <td>10:00:00 PM</td>
      <td>severe weather</td>
      <td>heavy wind</td>
      <td>NaN</td>
      <td>3000.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>10.87</td>
      <td>8.19</td>
      <td>6.07</td>
      <td>8.15</td>
      <td>1467293</td>
      <td>1801683</td>
      <td>1951295</td>
      <td>5222116</td>
      <td>28.09767152</td>
      <td>34.50101453</td>
      <td>37.36598344</td>
      <td>2300291</td>
      <td>276463</td>
      <td>10150</td>
      <td>2586905</td>
      <td>88.9206</td>
      <td>10.6870</td>
      <td>0.3924</td>
      <td>50447</td>
      <td>47287</td>
      <td>1.066825978</td>
      <td>2.7</td>
      <td>4571</td>
      <td>267895</td>
      <td>1.706265514</td>
      <td>2.1</td>
      <td>5310903</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>2010-10-26 20:00:00</td>
      <td>2010-10-28 22:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012</td>
      <td>6</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>Tuesday, June 19, 2012</td>
      <td>4:30:00 AM</td>
      <td>Wednesday, June 20, 2012</td>
      <td>11:00:00 PM</td>
      <td>severe weather</td>
      <td>thunderstorm</td>
      <td>NaN</td>
      <td>2550.0</td>
      <td>NaN</td>
      <td>68200.0</td>
      <td>11.79</td>
      <td>9.25</td>
      <td>6.71</td>
      <td>9.19</td>
      <td>1851519</td>
      <td>1941174</td>
      <td>1993026</td>
      <td>5787064</td>
      <td>31.99409925</td>
      <td>33.54333043</td>
      <td>34.43932882</td>
      <td>2317336</td>
      <td>278466</td>
      <td>11010</td>
      <td>2606813</td>
      <td>88.8954</td>
      <td>10.6822</td>
      <td>0.4224</td>
      <td>51598</td>
      <td>48156</td>
      <td>1.071476036</td>
      <td>0.6</td>
      <td>5364</td>
      <td>277627</td>
      <td>1.932088738</td>
      <td>2.2</td>
      <td>5380443</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>2012-06-19 04:30:00</td>
      <td>2012-06-20 23:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>1.2</td>
      <td>warm</td>
      <td>Saturday, July 18, 2015</td>
      <td>2:00:00 AM</td>
      <td>Sunday, July 19, 2015</td>
      <td>7:00:00 AM</td>
      <td>severe weather</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1740.0</td>
      <td>250</td>
      <td>250000.0</td>
      <td>13.07</td>
      <td>10.16</td>
      <td>7.74</td>
      <td>10.43</td>
      <td>2028875</td>
      <td>2161612</td>
      <td>1777937</td>
      <td>5970339</td>
      <td>33.9825762</td>
      <td>36.20585029</td>
      <td>29.77949828</td>
      <td>2374674</td>
      <td>289044</td>
      <td>9812</td>
      <td>2673531</td>
      <td>88.8216</td>
      <td>10.8113</td>
      <td>0.3670</td>
      <td>54431</td>
      <td>49844</td>
      <td>1.092027125</td>
      <td>1.7</td>
      <td>4873</td>
      <td>292023</td>
      <td>1.668704177</td>
      <td>2.2</td>
      <td>5489594</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>2015-07-18 02:00:00</td>
      <td>2015-07-19 07:00:00</td>
    </tr>
  </tbody>
</table>
<br />
</div>
---
### UNIVARIATE ANALYSIS

We used plotly.express.choropleth to get an idea of the **geographical distribution of our OUTAGE.DURATION⏱ data.** We mapped out the mean durations of power outages onto their respective states and noticed that **higher means were typically distributed among more populated/urbanized areas.**

<iframe src="assets/choroduration.html" width=800 height=600 frameBorder=0></iframe>

In this choropleth of mean outage duration, it seems Wisconsin has the greatest mean outage duration while the central United States as the lowest mean outage duration 

Perhaps there was a causal explanation for the distribution here, but we couldn't say for certain. Regardless, it gave us an inkling that perhaps we should see **how CUSTOMERS.AFFECTED🚶 was spread geographically and see if there was similarity**, since more densely-populated areas tend to have more people that necessitate electricity as a utility.

<div style="text-align:center"> 
<iframe src="assets/chorocustomers.html" width=900 height=600 frameBorder=0></iframe>
</div>

In this choropleth of mean customers affected, it seems Florida has the greatest mean customers affected. This could be due to the greater population of these states.

And sure enough, it seemed as though similar regions (such as New York or California) were decently dense with dissatisfied customers. Now it was time to compare them against each other.

<br />

---
### BIVARIATE ANALYSIS

To start, we took the raw data from CUSTOMERS.AFFECTED🚶 and plotted it against OUTAGE.DURATION⏱ as a scatterplot.

<iframe src="assets/individualscatter.html" width=800 height=600 frameBorder=0></iframe>

Oh... that doesn't look that good.

Not only was the relationship relatively non-linear, but there were clearly outliers mixed up in the noise of **~1500 outages**. To our dismay, we noticed a slightly decreasing trend instead of an increasing one, which was expected. 

However, when we stratified our data by state and **plotted the means per state instead, we reached a strange conclusion.**

<iframe src="assets/meanscatter.html" width=800 height=600 frameBorder=0></iframe>

Now there was a *much* more clear positive linear trend. Why do these two plots show opposite conclusions? We can chalk a lot of it up to **Simpson's Paradox: where data on the individual level tells a different story on a grouped level.**

On the individual level, outages that affect more and more people take less time to correct; speculating, this may have something to do with more widespread outages getting greater **sense of urgency and attention** in their correction.

On the state level, outages that affect more and more people take much longer to correct; this could be due to the fact that on the whole more populous states have more **complex power grids and systems** that take professionals significantly longer to diagnose the problem.

<br />

---
### INTERESTING AGGREGATES

We started thinking more about the causes behind these outages and *why* they were occuring. So, we crafted a pivot table with the each of the 50 states as rows and each of the six listed causes (conveniently found in a column titled CAUSE.CATEGORY) as columns. The values of the boxes inside would be the total counts of how many of which causes sparked outages in which states.

It's pretty massive, but here's the table:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>CAUSE.CATEGORY</th>
      <th>equipment failure</th>
      <th>fuel supply emergency</th>
      <th>intentional attack</th>
      <th>islanding</th>
      <th>public appeal</th>
      <th>severe weather</th>
      <th>system operability disruption</th>
    </tr>
    <tr>
      <th>POSTAL.CODE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AK</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>AL</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>AR</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>AZ</th>
      <td>4.0</td>
      <td>0.0</td>
      <td>15.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>CA</th>
      <td>21.0</td>
      <td>10.0</td>
      <td>24.0</td>
      <td>28.0</td>
      <td>9.0</td>
      <td>67.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>CO</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>CT</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>DC</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>DE</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>37.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>FL</th>
      <td>4.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>26.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>GA</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>16.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>HI</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>IA</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>ID</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>IL</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>40.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>IN</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>24.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>KS</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>KY</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LA</th>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>MA</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>MD</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>25.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>32.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>ME</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>MI</th>
      <td>3.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>83.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>MN</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>11.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>MO</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>11.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>MS</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>MT</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>NC</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>30.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>ND</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>NE</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>NH</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>NJ</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>NM</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>NV</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>NY</th>
      <td>2.0</td>
      <td>12.0</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>33.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>OH</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>26.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>OK</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>15.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>OR</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>19.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>PA</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>48.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>SC</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>SD</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>TN</th>
      <td>2.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>20.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>TX</th>
      <td>5.0</td>
      <td>3.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>17.0</td>
      <td>64.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>UT</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>35.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>VA</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>32.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>VT</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>WA</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>62.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>20.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>WI</th>
      <td>0.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>WV</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>WY</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>


One interesting thing you'll notice is that "severe weather" is the most common cause of outages *by far.* It is the only column in this table that where **more than one state has a number of outages in the double digits.**

A second thing worth note is that there are a lot of zeroes. This indicates that **not all states frequently experience all causes for specific outages.** In other words, certain states are more predisposed to certain susceptibilities than others, which lines up with most peoples' anecdotal knowledge of certain cities or states being worse for power outages.

---


## ASSESSMENT OF MISSINGNESS 🔋

Data can be absent from a dataset for many reasons. Sometimes it's intentional, other times it's not. Typically, it is good to assess the type of missingness in a dataset before taking your next steps.

### NMAR ANALYSIS

NMAR (or not Not Missing At Random) is a type of missingness characterized by data in a column being missing because of the value of the missing data itself. 

For example: If you get a bunch of missing values after administering a self-reported survey with the sole question "Rate your happiness on a scale of 1 to 10", it's probably the twos and threes that are missing from your dataset, because less happy individuals are more inclined to be too embarassed to report anything at all. Since the data that's missing is dependent on if that number is low, the missingness in the data here is NMAR.

In our power outage data, we could consider missing values in the CAUSE.CATEGORY.DETAIL🌪️ column as NMAR. The purpose of CAUSE.CATEGORY.DETAIL🌪️ is to supply extra information about the cause of an outage if necessary. It's likely the case that information was left missing from the column if that extra information was not considered relevant or necessary. 

Though we cannot know for sure if that is the exact criteria for whether or not missing values were left unattended, the fact that such a practice is likely tells us that **this data can be interpreted as NMAR.**

The missingness of the CAUSE.CATEGORY.DETAIL🌪️ column is dependent on whether or not the extra description is relevant and necessary.

<b>Further ahead, we will conduct a permutation test to reveal that, additionally, CAUSE.CATEGORY.DETAIL🌪️ is MCAR (Missing Completely At Random) with respect to another column.</b>

<br/>

---
### MISSINGNESS DEPENDENCY

Our last comparison of CUSTOMERS.AFFECTED🚶 and OUTAGE.DURATION⏱ was only made possible by dropping rows with missing values from the dataset, but how do we know that dropping those values didn't significantly impact our conclusion? Moreover, if we were to fill in (or impute) these missing values, how would we know *that* wouldn't significantly impact our conclusion?

**We need to assess whether or not data in our CUSTOMERS.AFFECTED🚶 column is MAR (Missing at Random) or MCAR (Missing Completely At Random) with respect to OUTAGE.DURATION⏱.**

- If our data winds up MCAR, we can be more liberal with how we tackle the data, because "Missing Completely At Random" means tweaking data at random **won't significantly change our distribution of OUTAGE.DURATION⏱ data.**

- If our data winds up MAR, we have to be more careful with how we tackle the data, because "Missing At Random" means tweaking data at random ***might* significantly change our distribution of OUTAGE.DURATION⏱ data.**

Here's a diagram of two distributions, one of OUTAGE.DURATION⏱ but only when CUSTOMERS.AFFECTED🚶 **has missing data**, and another of OUTAGE.DURATION⏱ but only when CUSTOMERS.AFFECTED🚶 **has present data**:

<iframe src="assets/dist1.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/dist2.html" width=800 height=600 frameBorder=0></iframe>

<br>

All things considered, the distributions have roughly the same shape; both of them are **right-skewed and lot of data is packed into that first bar.** This implies that our OUTAGE.DURATION⏱ data is of the same distribution regardless of whether or not CUSTOMERS.AFFECTED🚶 is absent, meaning *our data is probably MCAR.* But looks can be deceiving, so we more formally decided to run a permutation test and see what results lie waiting.

- <b> NULL HYPOTHESIS: </b> The distribution of OUTAGE.DURATION⏱ when CUSTOMERS.AFFECTED🚶 is missing **is the same** as the distribution of OUTAGE.DURATION⏱ when CUSTOMERS.AFFECTED🚶 is not missing.

- <b> ALT HYPOTHESIS: </b> The distribution of OUTAGE.DURATION⏱ when CUSTOMERS.AFFECTED🚶 is missing **is the NOT same** as the distribution of OUTAGE.DURATION⏱ when CUSTOMERS.AFFECTED🚶 is not missing.

- *Test statistic: the absolute mean difference in OUTAGE.DURATION⏱ for rows in which CUSTOMERS.AFFECTED🚶 was missing and rows in which CUSTOMERS.AFFECTED🚶 is not missing.*

- *Significance Level: 0.05*

Surprisingly, the p-value we recieved from running this test was ~0.02, which (at a standard α = 0.05 significance level) **rejects the null hypothesis and implies significant difference in the missing and non-missing distributions, or MAR dependence.**

It's a good thing we checked! It turns out we need to be more careful with our data.

---

However, as demonstrated above, the missingness of a column **might not depend on another column.** 

<b>Remember that NMAR column: CAUSE.CATEGORY.DETAIL🌪️?</b> 

In conjunction with data from OUTAGE.DURATION⏱, we conducted a permutation test to determine whether the missingness of CAUSE.CATEGORY.DETAIL (which contains 471 missing values) depends on OUTAGE.DURATION⏱. Note that for this permutation test, we removed rows for which OUTAGE.DURATION⏱ (the independent variable) had missing values.

- <b>Null Hypothesis: </b> The missingness of CAUSE.CATEGORY.DETAIL🌪️ **does not depend** on OUTAGE.DURATION⏱.

- <b>Alternative Hypothesis: </b>The missingness of CAUSE.CATEGORY.DETAIL🌪️ **does depend** on OUTAGE.DURATION⏱.

- *Test statistic: the absolute mean difference in OUTAGE.DURATION⏱ for rows in which CAUSE.CATEGORY.DETAIL🌪️ was missing and rows in which CAUSE.CATEGORY.DETAIL🌪️ is not missing.*

- *Significance Level: 0.05*

We recieved a p-value of ~0.08 for this comparison, which (at a standard α = 0.05 significance level) **fails to reject the null hypothesis and implies that CAUSE.CATEGORY.DETAIL🌪️ is not dependent on OUTAGE.DURATION⏱. In other words, CAUSE.CATEGORY.DETAIL is MCAR with respect to OUTAGE.DURATION⏱.** 

<br>

---

## HYPOTHESIS TESTING 🔋

With our data assessed and undestood, it was time to use OUTAGE.DURATION⏱ to address our big question:

> ## Every state has power outages from time to time, but which states' outages appear to be unusually long?

In our hypothesis tests, we investigated whether each of the fifty states had **longer power outages (duration) compared to the average power outage duration of the greater United States.** We defined long power outages as power outages that were greater than the national average duration. Thus, in order to simulate our null hypothesis, we created a boolean column of ***Trues*** for longer than the average duration and ***Falses*** for not longer than the average duration for each observation. 

Our hypotheses for each of the 49 tests (Alaska had no data) were as follows:

- <b>Null Hypothesis: </b> The state’s proportion of long power outages **is equal to** proportion of long power outages of the greater United States

- <b>Alternative Hypothesis: </b> the state’s proportion of of long power outages **is greater than** the proportion of long power outages of the greater United States

- *Test Statistic: the proportion of long power outages* 

- *Significance Level: 0.05*

These choices of hypothesis fit our overall question wherein we are trying to determine which states **more commonly have longer-than-average power outages.** With this test, we hoped to find regional patterns of where long power outages may occur (perhaps even coinciding with our choropleth of average CUSTOMERS.AFFECTED🚶 per state from earlier on).
 
Since we conducted the hypothesis test for each state, we have 49 p-values: one for each state indicating the probability of seeing such an observed proportion of long power outages if the null hypothesis was indeed true. <b>We have stored our p-values in the following DataFrame:</b>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Obs. Prop. of Long Outages</th>
      <th>P-Value</th>
      <th>Reject Null</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>TN</td>
      <td>0.161290</td>
      <td>0.96503</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MA</td>
      <td>0.166667</td>
      <td>0.92396</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PA</td>
      <td>0.631579</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VT</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DC</td>
      <td>0.400000</td>
      <td>0.31555</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>MS</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6</th>
      <td>WY</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>7</th>
      <td>AZ</td>
      <td>0.200000</td>
      <td>0.88349</td>
      <td>False</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NC</td>
      <td>0.128205</td>
      <td>0.99408</td>
      <td>False</td>
    </tr>
    <tr>
      <th>9</th>
      <td>AR</td>
      <td>0.240000</td>
      <td>0.76261</td>
      <td>False</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NM</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>11</th>
      <td>WA</td>
      <td>0.168539</td>
      <td>0.99658</td>
      <td>False</td>
    </tr>
    <tr>
      <th>12</th>
      <td>FL</td>
      <td>0.377778</td>
      <td>0.11654</td>
      <td>False</td>
    </tr>
    <tr>
      <th>13</th>
      <td>NH</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>14</th>
      <td>NE</td>
      <td>0.250000</td>
      <td>0.74098</td>
      <td>False</td>
    </tr>
    <tr>
      <th>15</th>
      <td>IL</td>
      <td>0.204545</td>
      <td>0.91732</td>
      <td>False</td>
    </tr>
    <tr>
      <th>16</th>
      <td>LA</td>
      <td>0.394737</td>
      <td>0.09810</td>
      <td>False</td>
    </tr>
    <tr>
      <th>17</th>
      <td>CT</td>
      <td>0.277778</td>
      <td>0.61702</td>
      <td>False</td>
    </tr>
    <tr>
      <th>18</th>
      <td>UT</td>
      <td>0.024390</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>19</th>
      <td>SC</td>
      <td>0.500000</td>
      <td>0.16957</td>
      <td>False</td>
    </tr>
    <tr>
      <th>20</th>
      <td>CO</td>
      <td>0.142857</td>
      <td>0.94115</td>
      <td>False</td>
    </tr>
    <tr>
      <th>21</th>
      <td>KS</td>
      <td>0.285714</td>
      <td>0.64287</td>
      <td>False</td>
    </tr>
    <tr>
      <th>22</th>
      <td>OH</td>
      <td>0.357143</td>
      <td>0.19605</td>
      <td>False</td>
    </tr>
    <tr>
      <th>23</th>
      <td>MD</td>
      <td>0.344828</td>
      <td>0.19610</td>
      <td>False</td>
    </tr>
    <tr>
      <th>24</th>
      <td>SD</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>25</th>
      <td>CA</td>
      <td>0.161616</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>26</th>
      <td>IA</td>
      <td>0.375000</td>
      <td>0.40895</td>
      <td>False</td>
    </tr>
    <tr>
      <th>27</th>
      <td>NJ</td>
      <td>0.593750</td>
      <td>0.00028</td>
      <td>True</td>
    </tr>
    <tr>
      <th>28</th>
      <td>GA</td>
      <td>0.058824</td>
      <td>0.99667</td>
      <td>False</td>
    </tr>
    <tr>
      <th>29</th>
      <td>MN</td>
      <td>0.466667</td>
      <td>0.10533</td>
      <td>False</td>
    </tr>
    <tr>
      <th>30</th>
      <td>VA</td>
      <td>0.111111</td>
      <td>0.99717</td>
      <td>False</td>
    </tr>
    <tr>
      <th>31</th>
      <td>ND</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>32</th>
      <td>MT</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>33</th>
      <td>NV</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>34</th>
      <td>WI</td>
      <td>0.263158</td>
      <td>0.67072</td>
      <td>False</td>
    </tr>
    <tr>
      <th>35</th>
      <td>DE</td>
      <td>0.025000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>36</th>
      <td>HI</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>37</th>
      <td>ID</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>False</td>
    </tr>
    <tr>
      <th>38</th>
      <td>NY</td>
      <td>0.542857</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>39</th>
      <td>MO</td>
      <td>0.400000</td>
      <td>0.23960</td>
      <td>False</td>
    </tr>
    <tr>
      <th>40</th>
      <td>OR</td>
      <td>0.120000</td>
      <td>0.98675</td>
      <td>False</td>
    </tr>
    <tr>
      <th>41</th>
      <td>IN</td>
      <td>0.380952</td>
      <td>0.11703</td>
      <td>False</td>
    </tr>
    <tr>
      <th>42</th>
      <td>WV</td>
      <td>0.500000</td>
      <td>0.32267</td>
      <td>False</td>
    </tr>
    <tr>
      <th>43</th>
      <td>KY</td>
      <td>0.384615</td>
      <td>0.30343</td>
      <td>False</td>
    </tr>
    <tr>
      <th>44</th>
      <td>MI</td>
      <td>0.747368</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>45</th>
      <td>AL</td>
      <td>0.200000</td>
      <td>0.81494</td>
      <td>False</td>
    </tr>
    <tr>
      <th>46</th>
      <td>OK</td>
      <td>0.363636</td>
      <td>0.27760</td>
      <td>False</td>
    </tr>
    <tr>
      <th>47</th>
      <td>TX</td>
      <td>0.221311</td>
      <td>0.95585</td>
      <td>False</td>
    </tr>
    <tr>
      <th>48</th>
      <td>ME</td>
      <td>0.166667</td>
      <td>0.92353</td>
      <td>False</td>
    </tr>
  </tbody>
</table>


Looking at each p-value, **we can fail to reject the null hypothesis for a majority of the states (46 total states) as they all have p-values greater than 0.05.**

**However, for 4 states, we can reject the null hypothesis at the 5% significance level and conclude that Pennsylvania, Michigan, New York, and New Jersey do not have a proportion of long power outages equal to that of the greater United States.**

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Obs. Prop. of Long Outages</th>
      <th>P-Value</th>
      <th>Reject Null</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>PA</td>
      <td>0.631579</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>27</th>
      <td>NJ</td>
      <td>0.593750</td>
      <td>0.00026</td>
      <td>True</td>
    </tr>
    <tr>
      <th>38</th>
      <td>NY</td>
      <td>0.542857</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
    <tr>
      <th>44</th>
      <td>MI</td>
      <td>0.747368</td>
      <td>0.00000</td>
      <td>True</td>
    </tr>
  </tbody>
</table>

It’s interesting to note that all of these states are located in the Northeast and Midwest (for Michigan), perhaps suggesting there are regional factors at play impacting power outage duration. All of these are fairly high on our mean CUSTOMERS.AFFECTED🚶 plot, suggesting there is overlap in these two areas. 

<div style="text-align:center"> 
<iframe src="assets/chorocustomers.html" width=900 height=600 frameBorder=0></iframe>
</div>

Moreover, it is also interesting to note that Wisconsin did not appear on this list despite having the greatest mean power outage duration overall. This may be because while Wisconsin has the greatest mean outage duration, its proportion of long durations is actually on par with the proportion of long outages nationwide. However, Wisconsin’s long outages — when they do occur — may be longer compared to the other states.

<b>It is also important to note that with such multiple testing without corrections, there is an increased risk of committing a Type 1 error wherein we reject the null hypothesis when it is actually true. </b>

---

So, we have gathered that:

- Outage data has many factors and causes🌪️ that make it complex to predict, and investigating that data as individual points or as aggregates can reveal contrasting trends.

-  Four states deviate from what is expected of the distribution of the greater United States in terms of OUTAGE.DURATION⏱.

- There is some overlap in the states that more commonly have longer power outages and the states who have a high number of mean CUSTOMERS.AFFECTED🚶 per outage.

... each of which is a quite *illuminating conclusion.*

<br>

> ## "Sometimes the best lighting of all is a power failure." - Douglas Coupland. 💡

