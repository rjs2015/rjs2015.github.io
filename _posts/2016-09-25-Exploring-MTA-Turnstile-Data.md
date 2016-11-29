---
layout: post
title: "Analyzing MTA Turnstile Data"
date: 2016-09-25
categories: 
  - Explorations
description: none
image: http://media.nbcnewyork.com/images/1200*746/subway-map-half.jpg
image-sm:
---
I just completed my first week of the [Metis](http://www.thisismetis.com/data-science-bootcamps) data science bootcamp, in which we dived into the details of data analysis with python, pandas, matplotlib, seaborn, and other tools.  The program structures its curriculum around a few projects, the first of which (Project Benson) had us working with MTA turnstile data to help a fictional non-profit client manage street team deployment to optimize outreach.

#### The Challenge

The client in our project, WomenTechWomenYes (WTWY), works to grow the participation of women in the tech field. WTWY is interested in maximizing attendence (and subsequently, donations) at an upcoming gala, and has asked how data from the MTA can be used to help. The MTA releases data every week tracking entries and exits by subway station across NYC.  This data, broken out into days and hour blocks, can be leveraged for analysis of how foot traffic fluctuates throughout the city.  

#### Initial Insights

With a few years of data available, it seemed useful to start with an exploration of seasonal trends - both to help WTWY identify months with highest ridership, and to define a more manageable time period for our analysis.  Looking from 2015 to present day, there was a recurring trend of above average ridership from March-May, drawing focus to this time period in 2016.  It was also helpful to get perspective on the variation in ridership over the course of an average week.  Plotting daily ridership through Spring 2016 revealed a steep decline every weekend, with ridership peaking mid-week.

Shifting to ridership by station, the first big question was which stations saw the most traffic.  While the answer was intuitive, with Penn Station and Grand Central rising to the top, the disproportionate share of ridership captured by large stations was slightly surprising.  The top 100 stations (of 459, sorted by ridership) accounted for roughly 60% of total traffic, with the top 5 stations accounting for more than 10%.

![]({{ site.url }}/assets/Ridership_Share.png)

#### Outreach Strategy

While the 5 stations boxed above are all worth allocating resources to, we need a little more detail around how to activate for each one. Categorizing them based on similar traffic patterns seemed like the best way to devise effective targeting strategies.  One method of categorization could involve looking at traffic distribution by day of the week and time of day for each station.  Seaborn has a handy heatmap implementation that I found well-suited for this analysis.

![]({{ site.url }}/assets/Heatmaps.png)

The MTA records their ridership data by station mostly in four hour blocks.  While counts are usually recorded at 8am, 12pm, 4pm, 8pm, etc., stations also have instances of counts recorded at different times.  These times vary between stations, and are depicted on the x-axis.

The above heatmaps highlighted a distinction between commuter stations - like Grand Central, Penn Station, and Herald Square - that saw most of their traffic during the weekday mornings and evenings, and general purpose stations (14 St-Union Square and Times Square) that experienced a good share of their traffic in the afternoon and had a less steep dropoff on the weekends.  This distinction partly indicated that focus could be shifted away from commuter stations in the afternoon, and was corroborated by geographic visualizations (made in Tableau) showing exits more widely distributed during this time.

![]({{ site.url }}/assets/Geo_MTA.png)

So far, we've talked a lot about ridership, but don't really have a handle on potential conversion rates (i.e. % of street team conversations leading to gala signups).  Without a direct measure of conversion, variables like tech company presence and income by region could point towards other promising areas to target, within which ridership could inform the best stations to cover.  Using data from the Census Bureau and NY Office of the Comptroller, another category of stations (separate from commuter and general purpose) was identified in high-income residential areas and/or tech hubs, including Canal Street, Chambers Street, and 86th Street. There may be more room to add to this part of the analysis, as other secondary variables - like female-to-male employment ratios - could help round out this category of stations.

#### Future Possibilities

This was an interesting exercise in manipulating data and connecting different insights, and I think there is potential for further exploration down the line. At first glance, the data revealed some opportunities for specific event-oriented outreach efforts, like targeting entertainment venues around big events that see a spike in ridership.  Validating these relationships and searching for causality in ridership trends could lead to more nuanced outreach strategies.  

![]({{ site.url }}/assets/March_Madness.png)

Additionally, if data could be collected from a first round of outreach, simulations using conversion rates, desired attendence, and street team size as parameters could be run to build detailed hourly deployment plans with reliable signup projections.

This was a great project that really got me excited about the tools and methods used in data science - I'm looking forward to the rest of the program!
