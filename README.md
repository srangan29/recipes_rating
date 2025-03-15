# Easy Work Makes the Recipe Work (Or Does It?): An Exploration into Recipes and the Influence of Perceived Easiness
Author: Sahana Rangan

## Introduction

What kind of people do you think are the ones looking up recipes online?
Likely people who don't have as much experience cooking, right? Otherwise, they wouldn't need a recipe online. 
For this reason, it makes a lot of sense that there are many recipes tagged as "easy" on food.com, if only to grab the attention of all of us looking up recipes for quick meals. 
But are all these recipes labelled easy really as easy as they say? 
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

## Data Cleaning and Explanatory Data Analysis
### Data Cleaning
1. Left merge the recipes and interactions datasets together.
- There are two datasets, so doing this helps to have a singular dataset that addresses all reviews and ratings with the recipes they are from.

2. In the merged dataset, fill all ratings of 0 with np.nan. 
- The reason for this is that a 0.0 rating is akin to being a missing value in the context of our ratings system (which only goes from 1-5). By replacing ratings 0.0, we are able to avoid bias and ensure something such as an average rating is not potentially affected by something that is equivalent to "not rated".

3. Find the average rating per recipe, as a Series. Add this Series containing the average rating per recipe back to the recipes dataset through merging it.
- This helps to summarize a recipe's ratings overall as a recipe potentially has more than one rating.   

5. Add `is_easy` column, which addresses whether a recipe is tagged as easy or not.
- This is a boolean column checking if each recipe is tagged easy. 
- I obtain this by extracting it from the `tags` column, using a custom function that converts the `tags` column into a proper list as the tags themselves are simply a string of a list and not an actual list.

6. Convert the `nutrition` column into a proper list, as it is currently just a string.
7. Add `calories` column, which contains the amount of calories in each recipe. Make sure it is a float.
8. Add `sodium` column, which contains the amount of sodium in each recipe. Make sure it is a float.
- Creation of sodium and calorie columns is because those are two main things people tend to be concerned about in regards to food consumption as too much of either tends to be seen as a bad thing. 

This cleaned dataset is then used for the remaining of my analysis.

### Result
The final cleaned dataframe contains the following columns:

| Column Name | Description |
| ----------- | ----------- |
| `name` | Recipe name |
| `id` | Recipe ID|
| `minutes` | Minutes to prepare recipe |
| `contributor_id`| User ID who submitted this recipe |
| `submitted` | Date recipe was submitted |
| `tags` | Food.com tags for recipe |
| `nutrition` | Nutrition information |
| `n_steps` | Number of steps in recipe |
| `steps` | Text for recipe steps, in order |
| `description` | User-provided description |
|`user_id` | User ID |
| `recipe_id` | Recipe ID |
| `date` | Date of interaction |
| `rating` | Rating given |
| `review` | Review text |
| `avg_rating` | Average rating of recipe |
| `is_easy` | If recipe is tagged easy or not |
| `calories` | Amount of calories in recipe |
| `sodium` | Amount of sodium in recipe |

As there are too many columns, below:
The columns of cleaned dataframe, showing only the relevant ones to my analysis and investigation:
`name`, `id`, `minutes`, `n_steps`, `n_ingredients`, `rating`, `avg_rating`, `is_easy`, `calories`, `sodium`.

First few rows of cleaned dataframe:
| name                               |     id |   minutes |   n_steps |   n_ingredients |   rating |   avg_rating | is_easy   |   calories |   sodium |
|:-----------------------------------|-------:|----------:|----------:|----------------:|---------:|-------------:|:----------|-----------:|---------:|
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        4 |            3 | True      |      386.1 |       24 |
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        1 |            3 | True      |      386.1 |       24 |
| impossible macaroni and cheese pie | 275022 |        50 |        11 |               7 |        4 |            3 | True      |      386.1 |       24 |
| impossible rhubarb pie             | 275024 |        55 |         6 |               8 |        3 |            3 | False     |      377.1 |       13 |
| impossible seafood pie             | 275026 |        45 |         7 |               9 |        1 |            3 | False     |      326.6 |       27 |

### Univariate Analysis
As an easy recipe is meant to be easy, I focused on potential factors that could contribute to a recipe being tagged as such.

First, I examined the distribution of `n_steps`, the number of steps a recipe takes.
<iframe
  src="assets/fig_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The graph has a skew to the right, being centered at 7 steps (which is the highest peak of the plot).

I also examined the distribution of `n_ingredients`, the number of ingredients a recipe takes.
<iframe
  src="assets/fig_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
There is a similar shape to `n_steps`, being slightly skewed to the right.

### Bivariate Analysis
Because I was curious if people would rate easier recipes differently, I plotted the easier recipes' distribution of ratings against the non-easy recipes' distribution of ratings.
<iframe
  src="assets/fig_easy.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As apparent in the graph, both easy tagged recipes and recipes not tagged easy share similar distributions, albeit drastically different counts. These differences may be a result of the counts for how many recipes are tagged easy within the dataset but still intriguing to note.

### Interesting Aggregates
Some interesting aggregates I found were:
Even a recipe with 2 ingredients had a mean over 60 minutes, which may be a sign of an outlier, as recipes with 1 or 3 ingredients were significantly shorter.

| n_ingredients | minutes (mean) |
|----------: --------|
|1|   36.375  |
|2| 1315      |
|3|  184.586  |
|4|  222.684  |
|5|   65.8771 |
 
A table of means of the data for recipes tagged easy vs not easy:
|   is_easy |   n_steps |   n_ingredients |   calories |   sodium |
|----------:----------:----------------:-----------:---------:|
|False |  12.6056  |        10.7953  |    481.764 |  30.0692 |
| True |  8.15922 |         7.83345 |    374.83  |  28.6819 |

## Assessment of Missingness

### NMAR Analysis
Within the dataset, there are a few columns with missing values, including: `review`, `rating`, `description`, and `avg_rating`.
The `review` column is likely NMAR because it could just be that some people decided to only leave a review because they had something to say. Otherwise, it
would be more inconvenient to do that in addition to going through a recipe if they had no opinion. 
The `description` column could potentially have the same logic, as in the author just decided to leave it blank because they didn't feel the need to add a description.

### Missingness Dependency
Another column containing missing values is `description`. 
To test the missingness dependency of this column, I tested it against `contributor_id` and `minutes`. 

Utilizing a significance level of **0.05** for all tests.
> Examining `minutes` first:
**Null Hypothesis:** The missingness of description is not dependent on the cooking time of the recipe.
**Alternate Hypothesis:** The missingness of description is dependent on the cooking time of the recipe.
**Test Statistic**: Absolute difference of the mean in the cooking time of the distribution of the group where description is missing vs distribution of the group where description is not missing.

Performing a permutation test, the observed statistic is 42.275133564016954, resulting in a p-value of 0.458. Below is the empirical distribution of the TVD for the test.
<iframe
  src="assets/missing_min_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
By this result of a p-value > the significance level  of 0.05, I **fail to reject the null hypothesis** and conclude there is evidence to say missingness of description is not dependent on the cooking time of the recipe.

> Examinining `contributor_id`:
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

As ratings only consist of 5 ordinal values, it makes sense to treat them as a categorical variable rather than a quantitative one.
Therefore, I chose my test statistic as a TVD to compare the distribution of the two categorical variables (`rating` and `is_easy`).
The purpose of performing a permutation test is due to the fact I only have a sample of the data and want to determine if two samples ("easy" and "non-easy" recipes) are potentially from the same population or not. 

Performing a permutation test by shuffling the `is_easy` column 1000 times, the observed statistic is 0.006188466840828628, resulting in a p-value of 0.001. 
<iframe
  src="assets/perm_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

By this result and a significance level of 0.05, I **reject the null hypothesis** and conclude there is evidence to say there is a difference in how people rate "easy" recipes vs "non-easy" recipes.

## Framing a Prediction Problem
With the knowledge that easy vs non-easy recipes do in fact differ, I am now interested in **predicting if a recipe is tagged easy or not.** Perhaps, for easier searching, there could be an automatic tagging system with regards to "easy" recipes. Therefore, I will be performing a binary classification.

To have an optimal predictor, I would be focusing on F1-score because it would be inconvenient for viewers if recipes that should be tagged 'easy' are not tagged as such and when they choose to filter for a recipe tagged easy, those recipes are neglected. It would also be inconvenient if a recipe was falsely labeled as easy when it was not, as a viewer would be misguided to believing the recipe was in fact meant to be easy.

At the time of each prediction, I would know `n_ingredients`, `n_steps`, `calories`, `sodium` and `minutes` as these are all components of the initial recipe itself.

## Baseline Model
For my baseline model, I utilized a Random Forest Classifier with the following features: `n_steps` (quantitative) and `n_ingredients` (quantitative).
I utilized a QuantileTransformer on both pieces of data as the graphs in my Univariate Analyis showed that both held skewed distributions. I felt there is a neglible difference between a recipe with, for example, one ingredient vs three, or three steps vs five, and went with this transformation.

My F1-score for my baseline was 0.7448865249286579 which wasn't that bad for only two features, but had room for improvement. Considering F1-score takes into account accuracy, precision, and recall, it's quite a good starting point for being optimal. 

## Final Model
For my final model, I included three other features: `calories`, `sodium`, and `minutes`. 
The main reason for these features is that logically, the more time a recipe takes, the less viable it would be to be deemed as easy. Thus, I included cooking time or `minutes`.
Additionally, an `easy` recipe may not necessarily care as much for nutritional value. For this reason, `calories` and `sodium` could potentially also be a sign of a recipe tagged easy or not, as it may be an easy recipe does not have as much health factor tied into it (thus less calories or less sodium).


My Random Forest Classifier utilized the following parameters: max_depth=62,
n_estimators=100, random_state=42, the hyperparameter max_depth having been found using GridSearchCV.

Instead of a QuantileTransformer for `minutes`, `calories` and `sodium`, however, I utilized a RobustScaler. I felt it was more appropriate to handle nutritional information in a way that would standardize it while also handling the outliers, rather than transforming it into quantiles. For `minutes`, most minutes listed were in multiples of 5, so transforming into quantiles felt like it took away from the miniutual differences in whether a recipe was listed as 50, 55, or 45.

My F1-score of my final model was 0.9395431359503216, a significant improvement from my baseline as it is much closer to 1, meaning my precision and recall are also close to 1.

## Fairness Analysis
For my fairness analysis, I am focusing on recipes with cooking times under an hour vs recipes that have cooking times equal to or over an hour.
I choose to focus on F1-score because it's important to me that the model is correctly predicting if a recipe is easy when it is easy and also not falsely predicting a recipe as easy when it shouldn't be. By focusing on F1-score, I am able to ensure I account for both false positives and negatives.

**Null Hypothesis:**My model is fair. Its F1 score for recipes that have a cooking time under an hour and recipes with a cooking time equal to or over an hour are roughly the same, with any differences a result of random chance.
**Alternate Hypothesis:** My model is unfair. Its F1 score for recipes that have a cooking time under an hour is not the same as its precision for recipes equal to or over an hour.
**Test Statistic:** Absolute difference in F1 Scores between recipes with cooking times below an hour and recipes with cooking times above/equal to an hour.

After performing a permutation test 1000 times, the resulting p-value is 0.0. Using a 0.05 significance level, I **reject the null hypothesis**, which indicates that the model is unfair and predicts differently for recipes with cooking times under 60 minutes and for recipes with cooking times greater than or equal to 60.

