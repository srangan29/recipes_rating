# Easy Work Makes the Recipe Work (Or Does It?): An Exploration into Recipes and the Influence of Perceived Easiness
Author: Sahana Rangan

Final Project for DSC80 at UCSD.

## Introduction

What kind of people do you think are the ones looking up recipes online?
Likely people who don't have as much experience cooking, right? Otherwise, they wouldn't need a recipe online. 
For this reason, it makes a lot of sense that there are many recipes tagged as "easy" on food.com, if only to grab the attention of all of us looking up recipes for quick meals. 
But are all these recipes dubbed as easy really as easy as they say? 
For example, there are many recipes out there focusing on how you can make a cake or cookie in 3-ingredients or less, in 5-minutes or less. Since it can be hard to get many ingredients on short notice, especially if you just want something simple, these recipes have attracted a lot of attention.
Is this then what results in their higher ratings? Or is there something else?

All of these things have resulted in me wondering what factors ultimately lead and/or contribute to each recipe on food.com's authors' declaring and tagging their recipes' as easy, whether it's number of ingredients or something else not as apparent. Mainly, centering on this question for my project, **is there evidence that there are differences between a recipe tagged 'easy' versus a recipe not tagged easy?**
 
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
There are two datasets, so doing this helps us have a singular dataset that addresses all reviews and ratings with the recipes they are from.

2. In the merged dataset, fill all ratings of 0 with np.nan. (Think about why this is a reasonable step, and include your justification in your website.)
The reason for this is that a 0.0 rating is meaningless in the context of our ratings system (which only goes from 1-5). By replacing ratings 0.0, we are able to ensure an average rating is not potentially affected by something that is equivalent to "not rated".
3. Find the average rating per recipe, as a Series.

4. Add this Series containing the average rating per recipe back to the recipes dataset however you’d like (e.g., by merging). Use the resulting dataset for all of your analysis. (For the purposes of Project 4, the 'review' column in the interactions dataset doesn’t have much use.)

5. Add `is_easy` column, which addresses whether a recipe is tagged as easy or not.
This is a boolean column checking if each recipe is tagged easy as the tags themselves are simply a list
I obtain this by extracting it from the `tags` column, using a custom function 

With regards to investigating my topic, I focused on 
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates

---

## Assessment of Missingness

[TO DO]

## Hypothesis Testing

[TO DO]

---

## Framing a Prediction Problem

[TO DO]

---

## Baseline Model

[TO DO]

---

## Final Model

[TO DO]

---

## Fairness Analysis

[TO DO]

