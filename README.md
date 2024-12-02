# Nutrient-Analysis
# This is a thing

remote_theme: pages-themes/slate@v0.2.0
plugins:
- jekyll-remote-theme

### Statistical Analysis of Nutrients of Food and Their Taste

## Introduction

In this project I did analysis of thousands of different recipes from Food.com. The two datasets were originally scraped by Bodhisattwa Prasad Majumder, Shuyang L∗, Jianmo Ni, and Julian McAuley  of UCSD. The datasets contain much information; such as ingredients, time to prepare and more but in this analysis we will particularly be looking at the nutrition.

The nutrition of food is very important when it comes to taste. Over the past few decades it has come to light that many companies add extra sugar, salt, or fat to make their product taste better and in turn make people buy it again. In this analysis we will be looking at how sugar, salt, and fat affect the taste of food.

# Recipes Dataset

| Column Name | Description |
| -------- | -------- |
| 'nutrition' | Contains the values of the nutrients in a recipe |
| 'name' | Name of the recipe |
| 'id' | Id of the recipe |
| 'minutes' | Time to prepare the recipe |
| 'contributer_id' | Id of the person who submitted the recipe |
| 'submitted' | The date of the recipe submission |
| ‘tags' | Food.com tags for the recipe |
| 'n_steps' | Number of steps in the recipe |
| ‘steps’ | Text for recipe steps |
| ‘description’ | Description of the recipe from the contributor | 

# Reviews Dataset

| Column Name | Description |
| -------- | -------- |
| ‘date’ | Date of interaction |
| 'user_id’ | The id of the user |
| ‘recipe_id’ | Id of the recipe |
| ‘rating’ | Individual rating of the recipe |
| ‘review’ | The text of the given review |


## Data Cleaning and Exploring the Data

# Data Cleaning

All of the columns in both datasets apart from recipe_id, rating in the Reviews Dataset and nutrition, id from the recipes dataset will not help answer the question so they are dropped. From there we can merge the two datasets on recipe_id and id so that each rating is adjacent to its respective nutritional content. After that we break up the nutrition column into the following individual columns : calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)

From there we will take the average rating of each recipe and replace each rating with that average. This makes predictions more accurate and easier. There was also one null value that we will drop because it won't make a difference across the whole dataset. We also drop duplicates to avoid redundancy. We also drop recipes with above 5000 calories so it doesn’t skew predictions as much and considering recipes with more than 5000 calories are less than 1% of the dataset it won’t matter. 

Here is the head of the dataframe 

dfhead**

# Bivariate Analysis

I performed Bivariate Analysis on the dataset at multiple columns with the average rating columns

Pics**

As you can see as the levels of Sugar, Sodium, and Protein all increase the rating also increases.

