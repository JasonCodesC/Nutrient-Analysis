
# Statistical Analysis of Nutrients of Food and Their Taste

## Introduction

In this project I did analysis of thousands of different recipes from Food.com. The two datasets were originally scraped by Bodhisattwa Prasad Majumder, Shuyang L∗, Jianmo Ni, and Julian McAuley  of UCSD. The datasets contain much information; such as ingredients, time to prepare and more but in this analysis we will particularly be looking at the nutrition. We have a lot of data to look at, after cleaning we have 83,562 rows of information.

The nutrition of food is very important when it comes to taste. Over the past few decades it has come to light that many companies add extra sugar, salt, or fat to make their product taste better and in turn make people buy it again. In this analysis we will be looking at how sugar, salt, and fat affect the taste of food.

### Recipes Dataset

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

### Reviews Dataset

| Column Name | Description |
| -------- | -------- |
| ‘date’ | Date of interaction |
| 'user_id’ | The id of the user |
| ‘recipe_id’ | Id of the recipe |
| ‘rating’ | Individual rating of the recipe |
| ‘review’ | The text of the given review |


## Data Cleaning and Exploring the Data

### Data Cleaning

All of the columns in both datasets apart from recipe_id, rating in the Reviews Dataset and nutrition, id from the recipes dataset will not help answer the question so they are dropped. From there we can merge the two datasets on recipe_id and id so that each rating is adjacent to its respective nutritional content. After that we break up the nutrition column into the following individual columns : calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)

From there we will take the average rating of each recipe and replace each rating with that average. This makes predictions more accurate and easier. There was also one null value that we will drop because it won't make a difference across the whole dataset. We also drop duplicates to avoid redundancy. We also drop recipes with above 5000 calories so it doesn’t skew predictions and visualizations as much and considering recipes with more than 5000 calories are much less than 1% of the dataset it won’t matter. 

Here is the head of the dataframe 

dfhead**

### Bivariate Analysis

I performed Bivariate Analysis on the dataset at multiple columns with the average rating columns

Pics**

As you can see as the levels of Sugar, Sodium, and Protein all increase the rating also increases. It is worth to note that the relationships aren't exactly linear so our predictions will not be perfect because some points have different ratings but the same nutrients.

### Univariate Analysis

Protein pics**

The distribution shows that most recipes contain less than 30 grams of protein. Although protein is healthy it can cause some people gut digestions and just overall not taste as good so most recipes contain little protein.

### Interesting Aggregates

*table

I created a table ranging from low to high calories into the bins 0-450, 450-900, 900-1350, 1350-Max. This table shows the average calories and nutrients for the recipes in each given bin. We can see from this that protein and carbs seem to grow slowly as the calories increase compared to fat and sugar which grow much faster.

### Imputation

I chose not to impute any missing values because there was only one so it would have changed the output very minimally and was not worth doing.

## Framing a Prediction Problem

We are going to be predicting the rating of a recipe based off of the sugar, total fat, and salt content of the recipe. Initally for my baseline model I chose a regression model but for my final model there seemed to be no improvements so I ended up using a classifier. This is multiclass classification because there a five different responses the model could give. I used accuracy as my scoring because it is typically better for multiclass classification. At the time of prediction we will know all of the values we are training the data on so there are no restrictions.

## Baseline Model

My Baseline Model is a simple linear regression model. The features are the previously stated above total fat, sugar, and salt which are all quantatative features and because of that I did not need to encode or change the values. The model has a training Mean Squared Error of 1.1909 which is good considering when the nutrients are small there is high variance in the rating. The test Mean Squared Error is 1.18375 which is better than the testing accuracy and shows that our model is not overfitting.

## Final Model

For my Final Model I decided to add two new features, 'saturated fat (PDV)' and 'protein'. I decided to get rid of 'total fat' because a smaller subset may help predictions better. I decided to add protein because it will probably help us be able to predict lower ratings better based off what we saw in the univariate analysis.

I initally used built a pipeline including a ridge regression model. I first used the StandardScalar and QuantileTransfomer to normalize the features and create a normal distribution which I found helped predictions. I used GridSearchCV to find the best hyperparamters and then fit the model and obtained a training Mean Squared Error of 1.19077 and a testing Mean Squared Error 1.18370. This is better than our baseline model but not by enough so I decided to see if trying out classification would help. 

In my classification model I just used StandardScalar and the KNeighborsClassifier. I also grouped the avg_rating into buckets: 1, 2, 3, 4, 5. All of the ratings were rounded up so a value less than or equal to 1 would become 1 and so on. Using the all of the other same features as the other final model I again used GridSearchCV to determine that 49 neighbors was the best hyperparamter for the model. I also used StandardScalar and QuantileTransformer again. The model achieved a better Mean Squared Error than both the baseline model and the previous final model. The training was 1.16961 and the testing was 1.1667 and the accuracy of the model was 69.93%. I think these results are quite good, in the Bivariate analysis we saw that much of the data contained points with the same input values but vastly different output values. Due to the data behaving like that I think the classifer ended up working very well.