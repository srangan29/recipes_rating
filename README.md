# Easy Work Makes the Recipe Work (Or Does It?): An Exploration into Recipes and the Influence of Perceived Easiness
Final Project for DSC80 at UCSD.

## Introduction

What kind of people do you think are the ones looking up recipes online?
Likely people who don't have as much experience cooking, right? Otherwise, they wouldn't need a recipe online. 
For this reason, it makes a lot of sense that there are many recipes tagged as "easy" on food.com, if only to grab the attention of all of us looking up recipes for quick meals. 

There's many recipes out there focusing on how you can make a cake or cookie in 3-ingredients or less. Since it can be hard to get many ingredients on short notice, especially if you just want something simple, these recipes have attracted a lot of attention.
However, compared to more ingredient items, is it true that the rating would generally be higher for recipes with higher ingredient counts? 
I intend to explore this idea and see if it truly is believed by people on these recipe websites rating the foods that more ingredients = higher deserved rating. 

Cost effectiveness of getting to buy less ingredients has always been an appealing part of those types of recipes, but low cost does not good food make (or so many may believe).

As someone who personally has been scolded by their mother for not being able to cook (or refusing to do so), I've seen the appeal of all the Tiktoks or Youtube shorts with the ==easy== 5-ingredient make in 5 minute recipes.
So, I became curious about how food.com had food tags including "easy"! 

In all of these things, I wish to explore just what allows people to promote their recipes as easy or not, whether it's number of ingredients or something else not as apparent!

The dataset utilized in my exploration of these ideas consists of recipes and reviews that have been posted on food.com since 2008.

The initial data has been given split into two datasets: recipe and interactions.
`recipe` is a dataset that
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

[TO DO]
1. Left merge the recipes and interactions datasets together.
2. In the merged dataset, fill all ratings of 0 with np.nan. (Think about why this is a reasonable step, and include your justification in your website.)
The reason for this is that a 0.0 rating is meaningless in the context of our ratings system (which only goes from 1-5). By replacing ratings 0.0, we are able to ensure an average rating is not potentially affected by something that is equivalent to "not rated".
3. Find the average rating per recipe, as a Series.
4. Add this Series containing the average rating per recipe back to the recipes dataset however you’d like (e.g., by merging). Use the resulting dataset for all of your analysis. (For the purposes of Project 4, the 'review' column in the interactions dataset doesn’t have much use.)

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

