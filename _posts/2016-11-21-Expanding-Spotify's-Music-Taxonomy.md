---
layout: post
title: "Soundtrack: Expanding Spotify's Music Taxonomy"
date: 2016-11-21
categories: 
  - Applications
description: http://52.55.179.229:5000/
image: http://www.fitncookies.com/wp-content/uploads/2014/09/genres-on-spotify.png
image-sm:
---

I'm a fan of Spotify, and use their intuitive playlist taxonomy to find good tunes.  But, there have been times when the preassembled 'Workout', 'Chill', or other mood-themed playlists haven't included the artists in my rotation.  For my third Metis project, I wanted to build a playlist creator using classification models to extend Spotify's themes to the artists I like.

#### Motivation

People have different music preferences for the same occasion.  When it's time to focus on work or fall asleep, some people might opt for classical artists, whereas I feel that my favorite hip hop artists probably have some collection of tracks to accompany any activity.  There is, however, a common thread in songs for specific activities like working out or relaxing, usually rooted in audio features such as tonality, loudness, pace, etc.  

Spotify's thematic playlists (e.g. 'Workout', 'Chill') do a great job at capturing this commonality, but often I've wanted more of a filter than a predefined playlist - something where I can say "I'm on a Kanye binge, and want a 'Workout' playlist of his music to start my day, but a 'Chill' one to end it".  Luckily, I coule tackle this problem myself through classification models built on data from the Spotify and Gracenote APIs. :smiley:

#### Outline

To create thematic playlists, we need to classify songs into the themes to which they are most aligned.  Using Spotify's pre-existing song classifications and audio feature data for a set of songs, we can create models that predict probabilities of a song belonging to each theme.  These models will then return thematic alignments for any songs that we feed in, based on it's audio features. So, if we apply the models to an artist's body of music, and then sort the songs based on predicted alignment for the theme we're interested in, we'll get a playlist with their music filtered by a theme.

#### Data Collection

To gather a training set of categorized songs and their audio features, I used spotipy, the python wrapper for the Spotify API, which allowed for crawling of their playlist hierarchy.  These audio features include what seem to be internally engineered metrics based on audio analysis, like 'Danceability' and 'Valence', but also more easily observable traits like 'Key'.  Since the goal of the project was to extend Spotify's categorizations, I had a hunch that the features they provide for songs would be helpful.

But, I wanted to account for other elements as well, like genre, which was not available at the song level in Spotify.  For this, I turned to the Gracenote API, using pygn (another python wrapper).  Besides genre, this data included information on artist type (e.g. 'Male Group'), and some interesting mood/sentiment classifications that were derived internally through deep learning.  

As another feature engineering effort (and fun exploration), I scraped lyrics for the songs in my training set from azlyrics.com, and applied some topic modeling through NMF to try and extract lyrical components.  I found that curses seem to group together in one topic, and later discovered that these topics weren't improving model performance meaningfully.

#### Modeling

With the data compiled, I experimented with different models for classifying songs into five categories ('Chill', 'Workout', 'Sleep', 'Party', and 'Travel').  

A few challenges made this process difficult.  The classes were fairly imbalanced, with 'Sleep' and 'Travel' much smaller than the others, making it hard to predict these categories.  I briefly implemented oversampling to correct for this, but found that precision suffered.  In my model, I wanted to preserve precision, thinking that an upbeat 'Workout' song playing in the middle of a user's 'Chill' playlist could ruin the experience, whereas missing a potential good fit for a playlist was more tolerable.

There was also the issue of similarity between classes like 'Workout' and 'Party', which shared themes of fast-paced, energetic songs.  One way to remedy this would be to combine classes, but I wanted to provide more granularity where possible.  

Ultimately, I got the best performance from a Gradient Boosted Tree classifier, which likely captured the non-linear interactions between features that may underlie Spotify's thematic classifications.  The 'Chill' and 'Workout' classes were easier to predict, probably as a function of class size and intra-class homogeneity.

#### App Development

To put this model into production, I started by creating a database of songs, with audio features and information pulled from the Spotify and Gracenote APIs.  Since API queries take some time, I didn't want to leave this collection step for when users interacted with the app.  

The data was stored in a PostgreSQL database, as I anticipated scaling up to include many more artists and songs.  If you have any requests for artists to add, let me know!

The app was then built with Flask and d3.js, providing an interface for users to select a theme and add artists to their playlist.  Under the hood, the songs for these artists are fetched from the database, and thematic predictions are produced using the previously fitted models.  Then, results are sorted based on the user-specified theme, and the top 'n' playlist songs (the user determines the playlist size) are returned.

To help users manage and understand the composition of their playlist, I built a d3 radar chart visualization, with a blob that depicts the playlist's alignment to different themes.  As its content changes, the blob's skew and acuteness update to reflect how well the playlist fits each theme.  Designing this dashboard was one of the most rewarding parts of the process, and really helped visualize the notion of playlists capturing different sentiments.

#### Future Work

I hope to improve classification accuracy for smaller and overlapping themes down the line, and add some features to the app (like exporting playlists to Spotify).  [Try it out](http://52.55.179.229:5000/) (best viewed full-screen in Chrome) - I'd love to hear what you think!

