---
layout: post
title: "RecipeRecs: Ingredient-Based Recipe Recommendations"
date: 2016-11-14
categories: 
  - Applications
description: http://52.55.179.229:5001/
image: http://ce.unm.edu/assets/imgs/enrich/cooking-header
image-sm:
---

One of my favorite hobbies is cooking.  When looking for recipes, I gravitate towards those with ingredients or flavors similar to things I've enjoyed in the past. So, I was curious to see if NLP principles could help in my never-ending search for related recipes.

#### Motivation

Finding good recipes can be hard.  And when you finally find one, it's common to want alternatives that work with the same flavors to see what else can be done with ingredients you've used previously.

A basic set similarity metric between two recipes' ingredients can address part of this problem, but it won't fully capture the underlying flavor components from which recipes are assembled, missing the opportunity to suggest recipes that might have slightly different ingredients but deliver a similar flavor profile.  Putting on my data scientist cap, I decided to derive these components through a topic modeling NLP exercise, treating recipes as documents.

#### Data Collection

For this project, I needed to compile a collection of recipes and their ingredients to begin modeling the components that "generate" each recipe's ingredients.  I mostly do my searching on allrecipes.com, and given its extensive set of recipes, I decided to scrape my data from the site.  Using the awesome and efficient scraping package scrapy, I was able to quickly grab general recipe information, ingredients, directions, and reviews for over 50K recipes, opening the door for some pretty interesting analyses.  Since the available fields varied by recipe and everything was in the form of unstructured text, I leveraged MongoDB to store the data.

To remove extraneous information like measurements from each ingredient, I used the handy ingredient-parser python package.  I also looked to remove some commonly occuring non-ingredient stop words (like 'thinly sliced'), and am still refining this part of the data cleaning process, as there are many unique instances of this.  

#### Modeling

After extracting the ingredient list from each recipe, I applied some standard NLP preprocessing techniques, including tokenizing and stemming, and compiled a TFIDF that measured the frequency of ingredients occuring in each recipe, controlling for ingredients that showed up in many recipes.  This was better suited to my task than CountVectorizer, because I wanted to reduce the impact of common ingredients like salt and pepper.

I was now able to experiment with NMF and find the flavor groupings that tied frequently co-occuring ingredients together.  I'm amazed at how well matrix decompostion is able to identify the foundational dimensions of data, and the results NMF produced here were pretty intuitive.

![]({{ site.url }}/assets/ingredient_topics.png)

NMF outperformed other topic modeling alternatives like LDA, and I was able to see clear groupings of flavors and ingredients with similar use cases.

This model also produced measures of recipe alignment to each flavor component, which provided a way to compare recipes. So, we could pass any recipe through the NMF model, and use a metric like cosine similarity to find the closest matches in a pre-existing database.

#### App Development

I wanted to provide recommendations for any recipe a user found on allrecipes.com, and so in my app I developed a scraper that would retreive recipe ingredients from the user-entered link.  Then, the saved NMF model would be applied to the recipe, and similarity would be calculated against recipes in the database I had compiled from allrecipes.com.  

It was also useful to visualize how each recipe was aligned with the different ingredient categories (or 'components'), and so I used d3.js to create a grouped bar chart comparing recipes across these dimensions.  The app was built in Flask, incorporating this chart and the recipe recommendations.

#### Future Work

I had the chance to experiment with some mini projects along the way, including an attempt to identify ingredient substitutes through user comments.  I was able to implement a Word2Vec model that effectively identified words and abbreviations signaling 'substitution', but given the many different ways users referenced ingredients, I would like to spend more time designing an approach to robustly isolate substitutes.  

There are plenty of other opportunities for exploratory analysis here - [this app](http://52.55.179.229:5001/) (best viewed full-screen in Chrome) is just one example of what we can cook up with recipe data!
