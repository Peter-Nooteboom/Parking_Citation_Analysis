# Parking Citation Revenue Prediction
---

![Parking Citation Revenue Prediction](https://i.imgur.com/pq7vCxx.png)

In this project I examined average daily parking citations issued in Los Angeles, as well as the revenue that can be attributed to those citations. Additionally, I trained an XGBoost time series model to predict average daily citation revenue in the forthcoming quarter.

> *Note:* This project was completed as part of the larger [Lucky Parking](https://github.com/hackforla/lucky-parking) project from [Hack for LA](https://www.hackforla.org/). 

## Parking Citations
In 2019, revenue generated from parking citations accounted for approximately 1.5%, of Los Angeles’s $10 billion budget. Even at that small percentage, parking citations bring in hundreds of millions of dollars for the city. However, on a day-to-day and quarter-to-quarter basis, there are a number of global, local, and seasonal factors that can lead to unpredictability in revenue attributable to those citations.

**Problem:** Predictable revenue is a consistent budgeting constraint that cities must grapple with. One way to increase citation revenue predictability is through quotas that a parking officer must reach. However, these quotas are anti-consumer. A better method for understanding and predicting parking citation revenue would allow for city officials to make more informed decisions and guidelines about parking regulation and enforcement that both helps generate reliable revenue for the city, but also is pro-consumer.

**Goal:** The goal of the following project is to examine the longitudinal trends of parking citations and daily parking revenue, and to use this information to inform the training of a time series projection model to predict daily citation revenue for the forthcoming quarter.

## Data
The data for the present project includes all parking citations issued from the Los Angeles Department of Transportation over the past several years. A regularly updated version of this data can be found on [data.lacity.org](https://data.lacity.org/Transportation/Parking-Citations/wjz9-h9np), or a snapshot of the data used can be found on in this repository.

The data includes information on approximately 14.5 million parking citations. For the purposes of this project data was selected from January 2015 through March 2022. Fine amount, date issued, time issued were isolated and cleaned. Then these variables were used to generate variables for year, month, day of the year, day of the month, day of the week, week of the year, holidays, weekends, hour of the day, and minute of the hour. For the purposes of the time series model, day of year, and year were scaled down as to not lead to over-weighting of those variables. The final data used for analyses included information on 14,318,250 citations

> *Note:* For the purposes of this project, revenue refers to the cumulative value of all citations issued during a given period of time. Actual final revenue that can be attributed to that period of time depends on final payments from cited individuals.

## LA Parking Citations Overview
With an initial examination of parking citation distributions, it appears that on average citations typically cost around $71, but range from $0 to rare instances in the low thousands. The median parking citations issued per day is a little under 7000. Together this average cost and volume results in a median cumulative daily revenue of slightly under $500,000. However, some days result in very little revenue, while others even surpass $1,000,000.

![LA Parking Citations Overview](https://i.imgur.com/CSOYztF.png)

## Daily Citation Revenue
With an overall view of daily revenue from January 2015 through March 2022, it appears that there is a fairly consistent pattern with regular peaks and valleys from ~$600,000 to ~$100,000. Most notably, we can see significant anomalies during 2020, likely a result of the COVID-19 pandemic. With a slightly zoomed in 12 month view, we can see that there are regular “seasonality” trends in the data each week, and very few irregularities.

![Daily Citation Revenue](https://i.imgur.com/kuzkd2D.png)

## Years
Getting a better sense of how each represented year compares, it appears that 2015, 2016, and 2017 were remarkably similar in terms of average daily issued citations (~6000), as well as the average daily revenue attributable to those citations (~$420,000). In 2018 and 2019, a reduction of ~500 citations was seen resulting in a new average daily revenue attributable to those citations of ~$385,000. In 2020, we saw the largest drop down to a daily average of 4000 citations, and average daily revenue attributable to those citations of ~$310,000. In the years following 2020, we have begun to see a recovery back to the earlier baseline.

![Years](https://i.imgur.com/ciFPn2j.png)

## Months of the Year
Zooming in more, we can see that across the months of the year, the most daily citations and highest generated daily revenue occurs in Q1 of the year. A slight dip is seen in Q2 and early Q3. However, moving out of Q3 into Q4, we begin to see a slight rise again in both total daily citations, as well as daily revenue.

![Months of the Year](https://i.imgur.com/eSm75N5.png)

## Day of the Month
Even further, looking at the days of the month we see again a fairly consistent pattern of around 5000-5500 citation a day, and an average daily revenue around $390,000. However, both the 1st and the 25th days of the month see slightly lower volume and revenue than other days. This is potentially due to the impact of major holidays on these dates in some of the months.

![Day of the Month](https://i.imgur.com/XNScW1n.png)

## Holidays
Investigating the potential impact of holidays further, we can see that on average on US national holidays significantly fewer citations are issued, and significantly less revenue is attributable to those days. This is expected as the number of parking monitors that are employed on holidays is likely much less than on non-holidays.

![Holidays](https://i.imgur.com/7Ysy78u.png)

## Day of the Week
Variation between days begins to seen further when comparing days of the week. Beginning on Mondays we see on average around 6000 daily citations issued. This peaks around 7000 on Tuesday, and slowly decreases back to 6000 throughout the week to Friday. Moving into Saturday and Sunday, we see a significant drop to around an average of 2000 citations per day. This pattern of increases and decreases is also matched in average daily revenue. This decrease in the weekends can again likely be partially attributed to work hours for parking monitors. However, it may also be a result of differing parking restrictions on theses days.

![Day of the Week](https://i.imgur.com/K0IUbmJ.png)

## Week of the Year
Each year has 52-53 weeks. Averaging across 2015-2022, we can similar trends as described previously. Q1 have the highest volume and revenue, and another peak early Q3. We see occasional dips in average daily volume and average daily revenue in weeks 13, 22, 27, 36, 41, and 52/53. These are again likely accounted for by holidays.

![Week of the Year](https://i.imgur.com/xNiM4fg.png)

## Day of the Year
Looking again at an entire year, but instead comparing each of the 365-366 days, we see a relatively stable pattern, with large drops in daily volume and revenue on July 4th (Independence Day) and December 25th (Christmas).

![Day of the Year](https://i.imgur.com/A9YX3K4.png)

## Hour of the Day
Beyond the daily averages previously examined, hourly averages can also be evaluated. Looking at average hourly citations and average revenue attributable to those hours, we can see that early morning and late night have low volume and revenue. While the first half of the day accounts for the bulk of the citations. Additionally, we see three major peaks that likely are accounted for by parking restriction times.

![Hour of the Day](https://i.imgur.com/fA5cuuL.png)

## Minute of the Hour
Finally, within each hour, specific minutes can also be examined. Interestingly, the each hour typically starts off slow, possibly due to a slight grace period before a citation is issued. A peak number of citations is issued 5 minutes after the beginning of the hour. We then see a consistent and gradual decrease in volume and revenue through the remainder of the hour. However, we see outliers at each 5-minute mark. This is likely accounted for by rounding to the nearest 5-minute mark by the parking monitor.

![Hour of the Day](https://i.imgur.com/NZ2AQJL.png)

## Exploratory Analysis Key Takeaways
- Over the past 7 years, trends have been relatively consistent year over year, outside of COVID-19.
- The majority of citations are issued during weekdays, with weekends being much lower.
- Q1 has the highest average daily citation volume and average daily revenue
- Holidays result in significantly lower volume and lower revenue.

## Quarterly Time Series Prediction
Leveraging these key takeaways, I aimed to train a time series predictive model that can predict daily citation revenue for the forthcoming quarter (3 months). Given recent literature suggesting the benefit of gradient boosting methods for time series, as well as their ease of tuning, an XGBoost model was trained. Additionally, this type of model allows for easy multivariate inclusion of important factors such as holiday status. After assessing feature importance weights, year and day of year were scaled to not result in over-weighting. Additionally, grid-search hyperparameter searching was used to tune the model. The following visualization compares actual Q1 2022 values with values predicted from the trained model. The model resulted in an RMSE of 76,579.36, and a MAPE of 17.34%.

![Quarterly Time Series Prediction](https://i.imgur.com/5HiSINA.png)

## Conclusion
From this analysis, we can see that daily revenue attributable to parking citations follow a relatively predictable pattern that is impacted by global events, time of year, day of the week, holidays, and other time-based variables. When applied in a time series XGBoost model, these variables were able to inform predictions of daily citation revenue into the forthcoming quarter with relatively good accuracy. The model and analyses could be applied by city officials to help make projections of expected departmental revenue, as well as a guide for informing parking regulations and enforcement patterns. Based on this modeling and analyses, in service of generating higher and predictable revenue that is pro-consumer, city officials should focus on several key takeaways. First, holidays and weekends result in lower than expected revenue, more balanced patrolling and enforcement across all days equally would provide consumers with more predictable and fair patterns of enforcement. Additionally, given the declines from COVID-19, there is still room for increased enforcement to reach pre-pandemic levels of revenue that consumers were accustom to. Finally, leveraging the provided model would allow officials to estimate expected daily revenue for the upcoming quarter without having to resort to parking citation quotas. Together these points allow officials to make crucial policy, budgeting, and enforcement decisions that do not rely on anti-consumer practices.

---

If you enjoyed this project, please consider following me on [Twitter](https://twitter.com/Peter_Nooteboom) and [Linkedin](https://www.linkedin.com/in/peter-nooteboom/).
