
# Statistical Analysis of Nutrients of Food and Their Taste

## Introduction

In this project I did analysis on thousands of different recipes from Food.com. The two datasets were originally scraped by Bodhisattwa Prasad Majumder, Shuyang Li, Jianmo Ni, and Julian McAuley  of UCSD. The datasets contain much information; such as ingredients, time to prepare and more but in this analysis we will particularly be looking at the nutrition. We have a lot of data to look at, after cleaning we have 83,562 rows of information.

The nutrition of food is very important when it comes to taste. Over the past few decades it has come to light that many companies add extra sugar, salt, or fat to make their product taste better and in turn make people buy it again. In this analysis we will be looking at how the specific nutrition of sugar, salt, and fat, and other nutrients affect the taste of food.

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

All of the columns in both datasets apart from `recipe_id` and `rating` in the Reviews Dataset and `nutrition` and `id` from the recipes dataset will not help us in this quest so they are dropped. From there we can merge the two datasets on recipe_id and id so that each rating is adjacent to its respective nutritional content. After that we break up the nutrition column into the following individual columns : `calories` (#), `total_fat` (PDV), `sugar` (PDV), `sodium` (PDV), `protein` (PDV), `saturated fat` (PDV), and `carbohydrates` (PDV)

From there we will take the average rating of each recipe and replace each rating with that average. This makes predictions more accurate and easier. There was also one null value that we will drop because it won't make a difference across the whole dataset. We also drop duplicates to avoid redundancy. We also drop recipes with above 5000 calories so it doesn’t skew predictions and visualizations as much and considering recipes with more than 5000 calories are much less than 1% of the dataset it won’t matter. 

Here is the head of the dataframe 

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>calories</th>
      <th>total_fat</th>
      <th>sugar</th>
      <th>sodium</th>
      <th>protein</th>
      <th>saturated_fat</th>
      <th>carbs</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>95.3</td>
      <td>1.0</td>
      <td>50.0</td>
      <td>16.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>7.0</td>
      <td>5.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>143.5</td>
      <td>5.0</td>
      <td>25.0</td>
      <td>3.0</td>
      <td>10.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>5.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>182.4</td>
      <td>2.0</td>
      <td>50.0</td>
      <td>7.0</td>
      <td>11.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>5.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>658.2</td>
      <td>45.0</td>
      <td>151.0</td>
      <td>35.0</td>
      <td>24.0</td>
      <td>72.0</td>
      <td>29.0</td>
      <td>4.38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>228.2</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>15.0</td>
      <td>4.83</td>
    </tr>
  </tbody>
</table>
</div>


### Bivariate Analysis

I performed Bivariate Analysis on multiple columns within my dataset with the average rating column. Here are some scatter plots below:

<iframe
  src="pics/DS398SUGAR.html"
  width="800"
  height="445"
  frameborder="0"
></iframe>
<iframe
  src="pics/DS398SALT.html"
  width="800"
  height="445"
  frameborder="0"
></iframe>
<iframe
  src="pics/398FF.html"
  width="800"
  height="445"
  frameborder="0"
></iframe>
As you can see as the levels of Sugar, Sodium, and Protein all increase the rating also increases. It is worth to note that the relationships aren't exactly linear so our predictions will not be perfect because some points have different ratings but the same amount of nutrients. It also is worth to note that protein has some low rated outliers so there may be more of a negative correlation compared to Sugar and Sodium.

### Univariate Analysis

<iframe
  src="pics/file-name.html"
  width="800"
  height="445"
  frameborder="0"
></iframe>

The distribution shows that most recipes contain less than 30 grams of protein and is therefore right skewed. Although protein is healthy it can cause some people gut digestion issues and just overall not taste as good so most recipes contain little protein. 

### Interesting Aggregates

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>avg_calories</th>
      <th>avg_protein</th>
      <th>avg_sodium</th>
      <th>avg_total_fat</th>
      <th>avg_sugar</th>
      <th>avg_carbs</th>
    </tr>
    <tr>
      <th>calorie_bins</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Low_Cals</th>
      <td>227.76</td>
      <td>19.06</td>
      <td>18.61</td>
      <td>15.35</td>
      <td>37.94</td>
      <td>7.64</td>
    </tr>
    <tr>
      <th>Medium_Cals</th>
      <td>612.02</td>
      <td>54.08</td>
      <td>39.18</td>
      <td>48.46</td>
      <td>73.66</td>
      <td>17.69</td>
    </tr>
    <tr>
      <th>High_Cals</th>
      <td>1070.07</td>
      <td>82.71</td>
      <td>62.42</td>
      <td>93.48</td>
      <td>133.99</td>
      <td>29.13</td>
    </tr>
    <tr>
      <th>Very_High_Cals</th>
      <td>2222.19</td>
      <td>110.99</td>
      <td>109.98</td>
      <td>177.28</td>
      <td>449.81</td>
      <td>80.81</td>
    </tr>
  </tbody>
</table>
</div>


I created a table ranging from low to high calories into the bins 0-450, 450-900, 900-1350, 1350-Max. This table shows the average calories and nutrients for the recipes in each given bin. We can see from this that protein and carbs seem to grow slowly as the calories increase compared to fat and sugar which grow much faster.

### Imputation

I chose not to impute any missing values because there was only one so it would have changed the output very minimally and was not worth doing.

## Framing a Prediction Problem

We are initally going to be predicting the rating of a recipe based off of the `sugar`, `total_fat`, and `sodium` content of the recipe. Initally for my baseline model I chose a regression model but for my final model there seemed to be no improvements so I ended up using a classifier. This is multiclass classification because there a five different responses the model could give. I used accuracy as my scoring because it is typically better for multiclass classification. At the time of prediction we will know all of the values we are training the data on so there are no restrictions. We will also come to add two more features to the final model.

## Baseline Model

My Baseline Model is a simple linear regression model. The features are the previously stated above `total_fat`, `sugar`, and `sodium` which are all quantatative features and because of that I did not need to encode or change the values. The model has a training Mean Squared Error of 1.1909 which is good considering when the nutrients are small there is high variance in the rating. The test Mean Squared Error is 1.18375 which is better than the testing accuracy and shows that our model is not overfitting.

## Final Model

For my Final Model I decided to add two new features, `saturated_fat` and `protein`. I decided to get rid of `total_fat` because a smaller subset may help predictions better. I decided to add `protein` because it will probably help us be able to predict lower ratings better based off what we saw in the earlier analysis, I think the low concentrations of protein could also help us sift through the data better for a good model.

I initally used built a pipeline including a ridge regression model. I first used the `StandardScalar` and `QuantileTransfomer` to normalize the features and create a normal distribution which I found helped predictions. I used `GridSearchCV` to find the best hyperparamters and then fit the model and obtained a training Mean Squared Error of 1.19077 and a testing Mean Squared Error 1.18370. This is better than our baseline model but not by enough so I decided to see if trying out classification would help. 

In my classification model I first grouped the `avg_rating` into buckets: 1, 2, 3, 4, 5. All of the ratings were rounded up so a value less than or equal to 1 would become 1 and so on. Using the all of the other same features as the other final model I again used `GridSearchCV` to determine that 49 neighbors was the best hyperparamter for the model. I again put `StandardScalar` and `QuantileTransformer` into a pipeline with the classifier. The model achieved a better Mean Squared Error than both the baseline model and the previous final model. The training was 1.16961 and the testing was 1.1667 and the accuracy of the model was 69.93%. This error is smaller than our baseline model and thus a better model. I think these results are quite good, in the Bivariate analysis we saw that much of the data contained points with the same input values but vastly different output values. Due to the data behaving like that I think the classifer ended up working very well. 