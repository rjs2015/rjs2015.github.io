---
layout: post
title: "Exponentially Entertaining: Modeling Box Office Revenue"
date: 2016-10-17
categories: 
  - Explorations
description: 
image: http://ftp.gnome.org/mirror/addons.superrepo.org/v7/addons/plugin.video.mdfm/fanart.jpg
image-sm:
---
Three weeks into Metis, and we've got another project under our belt, this time using web scraping, pandas, and regressions to explore movie data.  Box Office Mojo keeps a pretty extensive record of weekly box office gross for movies, so I thought it would be interesting to dig into this.

#### Motivation

Interested in using the weekly data, I brainstormed some relevant business questions, and was drawn to the idea of estimating weekly box office figures for a movie given general information on the film and some basic stats from its opening weekend.  This seemed like an intersting notion for a few reasons.  

If studios could model a movie's expected theatrical longevity, they could better optimize their distribution schedules and marketing timelines.  Estimates for later weeks could also help refine goals for opening weekend. Even theaters could benefit from these models, deciding which movies to phase out based on expected performance "x" weeks from release.  

So, there seemed to be enough here to motivate an exploration of modeling weekly box office revenue.

#### Initial Insights

A basic exploration of trends in movies' weekly revenue uncovered a fairly consistent and extremely useful trend - the weekly figures for movies usually followed an exponential decay pattern.  This seems intuitive, as a movie's audience dwindles the further out we get from its release.  Moreover, so much emphasis is placed on release weekend, which came across clearly in the data.  

This observation had interesting implications for the modeling of weekly revenue.  If exponential decay describes nearly every movie's revenue, we can build a model to estimate the decay parameter "beta" (based on a movie's attributes, cast, etc.), and ultimately predict movie revenues for each week after release using an exponential decay model.  Another important assumption in this effort was that data on opening weekend gross would be available.  

To confirm the consistency of this observation, the below plot points out that around 65% of movie revenues can be estimated with less than 20% error by fitting a no-frills exponential decay model of the form: N0 * e^(-β * t).  

![]({{ site.url }}/assets/Errors.png)

For the other movies, the exponential decay model seems to be tripped up by a limited release, where a movie is in fewer theaters on its release date than at its widest distribution.  In fact, "release strategy", a measure of the ratio of theater count upon a movie's release to that for its widest distribution, is correlated with how well the simple exponential decay model fits a movie's data.

![]({{ site.url }}/assets/Theater_Share.png)

After the point of widest distribution, a movie's weekly returns tend to decay exponentially, falling in line with our assumption.  

#### Model Performance and Takeaways

After scraping data going back to 2005 from different sources, I created features for general information about each movie, critic and audience reviews, previous cast accolades, and opening weekend performance.  I then tested various models for the beta (decay) parameter, achieving the best performance with a Gradient Boosted Tree.  To clarify, the larger the beta, the faster a movie's revenues will decay.  

This proved to be a challenging exercise, one that can likely be improved in the future through collecting more data, designing a proxy for "hype" around a movie's release, and isolating the confounding effects of distributors/theaters deliberately phasing out a movie from the more "inherent" factors contributing to revenue decay.  

The most important features emerged as expected, with opening weekend performance (both financially and critically) being a strong determinant of beta, along with budget, genre, and actor accolades (Oscar noms/wins).  

![]({{ site.url }}/assets/Features.png)

This model provides an interesting framework for estimating movie revenues, and was a fun experiment in applying an alternate perspective to a common challenge.  I also am a huge fan of the Beautiful Soup and Selenium packages, and hope to use them on my other projects.


