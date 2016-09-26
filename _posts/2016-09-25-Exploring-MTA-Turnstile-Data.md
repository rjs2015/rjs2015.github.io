---
layout: post
title: "Analyzing MTA Turnstile Data"
date: 2016-09-25
categories: 
  - Explorations
description: 
image: http://media.nbcnewyork.com/images/1200*746/subway-map-half.jpg
image-sm:
---
I just completed my first week of the [Metis](http://www.thisismetis.com/data-science-bootcamps) data science bootcamp, in which we dived into the details of data analysis with python, pandas, matplotlib, and other tools.  The program structures its curriculum around a few projects, the first of which (Project Benson) had us working with MTA Turnstile Data to help a fictional non-profit client manage street team deployment to maximize outreach.

#### The Challenge

The client in our project, WomenTechWomenYes (WTWY), works to grow the participation of women in the tech field. WTWY is interested in maximizing attendence (and subsequently, donations) at an upcoming gala, and has asked how data from the MTA can be used to optimize outreach efforts. The MTA releases data every week tracking entries and exits by subway station across NYC.  This data, broken out into days and hour blocks, can be leveraged for analysis of how foot traffic fluctuates throughout the city.  

#### Initial Insights

With a few years of data available, it seemed useful to start with an exploration of seasonal trends - both to help WTWY identify months with highest ridership, and to define a more manageable time period for our analysis.  Looking back through 2015, there was a clear trend of above average ridership from March-May, drawing our attention to this time period in 2016.  It was also helpful to get perspective on the variation in ridership over the course of an average week.  Plotting daily ridership over our time period revealed a steep decline every weekend, with ridership peaking mid-week.

Shifting to ridership at the station level, the first big question was which stations saw the most traffic.  While the answer was intuitive, with Penn Station and Grand Central rising to the top, the disproportionate share of ridership captured by large stations was slightly surprising.  The top 100 stations (sorted by ridership) accounted for roughly 60% of total traffic, with the top 5 stations accounting for more than 10%.

![]({{ site.url }}/assets/Ridership_Share.png)
