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
Three weeks into Metis, and we've got another project under our belt, this time using web scraping, pandas, and regressions to explore movie data.  Box Office Mojo keeps a pretty extensive record of weekly box office gross, so I thought it would be interesting to dig into this a little further.

#### Motivation

Interested in using the weekly data, I brainstormed some relevant business questions, and was drawn to the idea of estimating weekly box office figures for a movie given general information on the film and some basic stats from its opening weekend.  This seemed like an intersting notion for a few reasons.  If studios could model a movie's expected theatrical longevity, they could better optimize their distribution schedules and marketing timelines.  Estimates for later weeks could also help refine goals for opening weekend.  Finally, even theaters could benefit from these models, deciding which movie's to phase out based on expected performance 'x' weeks from release.  

So, there seemed to be enough here to motivate an exploration of modeling weekly box office revenue.

#### Initial Insights

A basic exploration of trends in movies' weekly revenue uncovered a fairly consistent and extremely helpful trend - the weekly figures for movies usually followed an exponential decay pattern.  This seems intuitive, as a movie's audience dwindles the further out we get from its release.  Moreover, so much emphasis is placed on release weekend, which came across clearly in the data.  

This observation had interesting implications for the modeling of weekly revenue.  If we could fit an exponential decay to nearly every movie's revenue, and then build a model to estimate the decay parameter beta (based on a movie's attributes, cast, etc.), we might be able to predict movie revenues for each week after release.  Another important assumption in this effort was that data on opening weekend gross would be available.  

![]({{http://g.recordit.co/u9srgyhPb5.gif}})
