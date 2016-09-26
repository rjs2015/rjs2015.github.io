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

![Ridership Share](https://github.com/rjs2015/rjs2015.github.io/blob/master/_screenshots/Ridership_Share.png "Ridership Share")
