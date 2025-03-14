# Easy Work Makes the Recipe Work (Or Does It?): An Exploration into Recipes and the Influence of Perceived Easiness
Author: Sahana Rangan

## Introduction

What kind of people do you think are the ones looking up recipes online?
Likely people who don't have as much experience cooking, right? Otherwise, they wouldn't need a recipe online. 
For this reason, it makes a lot of sense that there are many recipes tagged as "easy" on food.com, if only to grab the attention of all of us looking up recipes for quick meals. 
But are all these recipes dubbed as easy really as easy as they say? 
For example, there are many recipes out there focusing on how you can make a cake or cookie in 3-ingredients or less, in 5-minutes or less. Since it can be hard to get many ingredients on short notice, especially if you just want something simple, these recipes have attracted a lot of attention.
Is this then what results in their higher ratings? Or is there something else?

All of these things have resulted in me wondering what factors ultimately lead and/or contribute to each recipe on food.com's authors' declaring and tagging their recipes' as easy, whether it's number of ingredients or something else not as apparent. Mainly, centering on this question for my project, **is there evidence that there are legitimate differences between a recipe tagged 'easy' versus a recipe not tagged easy?**
 
The dataset utilized in my exploration of these ideas consists of recipes and reviews that have been posted on food.com since 2008.

The initial data has been given split into two datasets: `recipe` and 
`interactions.`
`recipe` is a dataset that has 83782 rows and 12 columns.
`recipe`'s columns consist of the following: 

| Column Name | Description |
| ----------- | ----------- |
| `name` | Recipe name |
| `id` | Recipe ID|
| `minutes` | Minutes to prepare recipe |
| `contributor_id`| User ID who submitted this recipe |
| `submitted` | Date recipe was submitted |
| `tags` | Food.com tags for recipe |
| `nutrition` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `n_steps` | Number of steps in recipe |
| `steps` | Text for recipe steps, in order |
| `description` | User-provided description |

`interactions` is a dataset that has 731927 rows and 5 columns.
`interactions`'s columns consist of the following:

| Column Name | Description |
| ----------- | ----------- |
| `user_id` | User ID |
| `recipe_id` | Recipe ID |
| `date` | Date of interaction |
| `rating` | Rating given |
| `review` | Review text |

---

## Data Cleaning and Explanatory Data Analysis
### Data Cleaning
1. Left merge the recipes and interactions datasets together.
There are two datasets, so doing this helps to have a singular dataset that addresses all reviews and ratings with the recipes they are from.

2. In the merged dataset, fill all ratings of 0 with np.nan. 
The reason for this is that a 0.0 rating is meaningless in the context of our ratings system (which only goes from 1-5). By replacing ratings 0.0, we are able to ensure an average rating is not potentially affected by something that is equivalent to "not rated".

3. Find the average rating per recipe, as a Series.
This sums 

4. Add this Series containing the average rating per recipe back to the recipes dataset however you’d like (e.g., by merging). Use the resulting dataset for all of your analysis. (For the purposes of Project 4, the 'review' column in the interactions dataset doesn’t have much use.)

5. Add `is_easy` column, which addresses whether a recipe is tagged as easy or not.
This is a boolean column checking if each recipe is tagged easy as the tags themselves are simply a list
I obtain this by extracting it from the `tags` column, using a custom function 

6. extraction of sodium and calorie columns because those are two main things people tend to be concerned about in regards to food consumption as too much of either tends to be seen as a bad thing.
This could potentially also be a sign of a recipe tagged easy or not, as it may be an easy recipe does not have as much health factor tied into it (thus less calories or less sodium).


### Result
The columns of cleaned dataframe:


First few rows of cleaned dataframe:
| name                               |     id |   minutes |   n_steps |   n_ingredients |   rating | is_easy   |   calories |   sodium |
|:-----------------------------------|-------:|----------:|----------:|----------------:|---------:|:----------|-----------:|---------:|
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        4 | True      |      386.1 |       24 |
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        1 | True      |      386.1 |       24 |
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        4 | True      |      386.1 |       24 |
| impossible rhubarb pie             | 275024 |        55 |         6 |               8 |        3 | False     |      377.1 |       13 |
| impossible seafood pie             | 275026 |        45 |         7 |               9 |        1 | False     |      326.6 |       27 |

With regards to investigating my topic, I focused on a few columns I found potentially relevant to the easiness of a recipe. 
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates

---

## Assessment of Missingness

### NMAR Analysis
The `review` column is likely NMAR because it could just be that some people decided to only leave a review because they had something to say. Otherwise, it
would be more inconvenient to do that in addition to going through a recipe if they had no opinion. 

### Missingness Dependency
Another column containing missing values is `description`. 
To test the missingness dependency of this column, I tested it against `contributor_id` and `minutes`. 

Utilizing a significance level of **0.05** for all tests.
- Examining `minutes` first:
**Null Hypothesis:** The missingness of description is not dependent on the cooking time of the recipe.
**Alternate Hypothesis:** The missingness of description is dependent on the cooking time of the recipe.
**Test Statistic**: Absolute difference of the mean in the cooking time of the distribution of the group where description is missing vs distribution of the group where description is not missing.

Performing a permutation test, the observed statistic is 42.275133564016954, resulting in a p-value of 0.452. Below is the empirical distribution of the TVD for the test.
<iframe
  src="assets/missing_min_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
By this result of a p-value > the significance level  of 0.05, I **fail to reject the null hypothesis** and conclude there is evidence to say missingness of description is not dependent on the cooking time of the recipe.

- Examinining `contributor_id`:
**Null Hypothesis:** The distribution of contributor_id when description is missing is the same as the distribution of contributor_id when description is not missing.
**Alternate Hypothesis:** The distribution of contributor_id when description is missing is not the same as the distribution of contributor_id when description is not missing.
**Test Statistic**: TVD.

Performing a permutation test by shuffling the `description` column 1000 times, the observed TVD is 0.996449224334763, resulting in a p-value of 0.0. Below is the empirical distribution of the TVD for the test.
<iframe
  src="assets/missing_fig_contributorid.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

By the result of the p-value < significance level of 0.05, I **reject the null hypothesis** and conclude there is evidence to say the distribution of contributor_id when description is missing is not the same as the distribution of contributor_id when description is not missing.

## Hypothesis Testing
As said before, I wish to explore the differences between "easy" and "non-easy" recipes. Thus, one of these differences includes how people treat and rate recipes tagged easy versus non-easy recipes. 
To address this, I ran a permutation test with the following:
**Null Hypothesis:** There is no difference in how people rate "easy" recipes vs non-easy recipes.
**Alternate Hypothesis:** There is a difference in how people rate "easy" recipes vs non-easy recipes.
**Test Statistic**: TVD

As ratings only consist of 5 ordinal values, it makes sense to treat them as such.
Therefore, 
The purpose of performing a permutation test is due to the fact I only have a sample of the data and want to determine if two samples ("easy" and "non-easy" recipes) are from the same population or not. 

Performing a permutation test by shuffling the `is_easy` column 1000 times, the observed statistic is 0.006188466840828628, resulting in a p-value of 0.001. 

By this result and a significance level of 0.05, I **reject the null hypothesis** and conclude there is evidence to say there is a difference in how people rate "easy" recipes vs "non-easy" recipes.

---

## Framing a Prediction Problem
Predicting if a recipe is tagged easy or not.
Perhaps, for easier searching, there could be an automatic tagging system with regards to "easy" recipes. 
At the time of each prediction, I would know `n_ingredients`, `n_steps`, `calories`, `sodium` and `minutes` as these are all components of the recipe.

---

## Baseline Model
For my baseline model, I utilized a Random Forest Classifier with the following features: `n_steps` and `n_ingredients`

---

## Final Model
For my final model, I included three other features: `calories`, `sodium`, and `minutes`.
The main reason for these features is that the more time a recipe takes, the less viable it would be to be deemed as easy. For this reason, `calories` and `sodium`

---

## Fairness Analysis
I chose to focus on precision parity because it's more important to me that my model is correctly predicting that a recipe is easy when it is in fact easy because if a non-easy recipe is tagged improperly, it would be a misleading tag for people wanting to look at easy recipes and potentially a waste of someone's time. It would be better to have a smaller range of recipes properly labelled than ones not labelled properly. With this test, I want to check what differences could exist between recipes from heavy contributors to food.com versus recipes contributed from one-time contributors. 

**Null Hypothesis:** My model is fair. Its precision for recipes from people that have contributed multiple recipes and recipes from people that have only contributed 1 recipe are roughly the same, with any differences a result of random chance.
**Alternate Hypothesis:** My model is unfair. Its precision for recipes from people that have only contributed 1 recipe is lower than its precision for recipes from people that have contributed multiple recipes.
**Test Statistic:** Difference in precision between recipes from heavy contributors and recipes from one-time contributors.

After performing a permutation test 1000 times, the resulting p-value is . Using a 0.05 significance level, I **reject the null hypothesis**, which indicates that the model is 

