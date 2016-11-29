---
layout: post
title: "Expanding Spotify's Music Taxonomy"
date: 2016-11-07
categories: 
  - Explorations
description: http://52.55.179.229:5000/
image: http://www.fitncookies.com/wp-content/uploads/2014/09/genres-on-spotify.png
image-sm:
---

I'm a fan of Spotify, and use their intuitive playlist taxonomy to find good tunes.  But, there have been times when the preassembled 'Workout', 'Chill', or other mood-themed playlists haven't included the artists in my rotation.  For my third Metis project, I wanted to build a playlist creator using classification models to extend Spotify's themes to the artists I like.

#### Motivation

People have different music preferences for the same occasion.  When it's time to focus on work or fall asleep, some people might opt for classical artists, whereas I feel that my favorite hip hop artists probably have some collection of tracks to accompany any activity.  There is, however, a common thread in songs fit for things like working out or relaxing, usually rooted in audio features like tonality, loudness, pace, etc.  

Spotify's thematic playlists (e.g. 'Workout', 'Chill') do a great job at capturing this commonality, but often I've wanted more of a filter than a predefined playlist - something where I can say "I'm on a Kanye binge, and want a 'Workout' playlist of his music to start my day, but a 'Chill' one to end it".  Luckily, I coule tackle this problem myself through classification models built on data from the Spotify and Gracenote APIs :)


