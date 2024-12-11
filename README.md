# Sugar and Protein Influence on Recipe Ratings: An Investigation into Variance Differences

## Introduction
In this analysis, we explore the influence of nutritional components and recipe complexity on user ratings within a comprehensive dataset of recipes. This dataset not only details each recipe's name and preparation time but also enriches our study with a breakdown of nutritional content—specifically focusing on sugar, protein, and the total number of ingredients. By establishing a model to predict recipe ratings based on these variables, we aim to uncover patterns that may suggest how certain nutritional aspects and recipe complexity influence user preferences and ratings. This exploration seeks to provide insights into the dietary preferences that resonate most with users, potentially guiding healthier or more appealing recipe modifications

We are given two dataset for our project, `recipes.csv` and `interaction.csv` 

#### Recipes Datasets:

`name`: The name of the recipe. Useful for identification and user-friendly outputs in analysis.

`id`: Unique identifier for each recipe. Key for linking with user ratings and reviews.

`minutes`: Time required to prepare the recipe. This variable can be used to analyze time preferences in recipe ratings.

`contributor_id`: User ID of the individual who submitted the recipe. Allows us to track submissions by user but is less relevant for predictive modeling.

`submitted`: The date when the recipe was posted. Not directly used in rating predictions but helpful for understanding trends over time.

`tags`: Keywords associated with the recipe from Food.com, indicating cuisine, dietary considerations, and other descriptors that might influence user preferences.

`nutrition`: An array containing nutritional values such as calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates (all as percentage of daily value, PDV). Sugar and protein are of particular interest as predictive variables.

`n_steps`: The number of steps required to make the recipe. This can be an indicator of complexity and might affect user ratings.

`steps`: Detailed textual instructions for preparing the recipe. While verbose, text analysis could uncover insights into complexity and clarity’s effect on ratings.

`description`: A brief user-provided summary or description of the recipe, which could influence a user's decision to try or rate the recipe.

#### Ratings Dataset:

`user_id`: Identifier for the user providing the rating. Useful for personalized analysis but not directly used for overall rating predictions.

`recipe_id`: Connects the rating to the specific recipe in the Recipes dataset.

`date`: When the rating was posted. Provides temporal insights but typically not used in basic rating predictions.

`rating`: The score given to the recipe by a user. This is the target variable for your predictive model.

`review`: Textual review accompanying the rating. Potential for sentiment analysis and its correlation with numeric ratings.

### Exploring Culinary Preferences Through Data
Our project utilizes a detailed dataset from Food.com, encompassing a diverse range of recipes each tagged with comprehensive attributes like preparation time, nutritional values, and user-generated content including ratings and reviews. Key features such as the sugar and protein content of recipes are extracted from nutritional information to predict how these factors influence recipe ratings. Each recipe is also described through its preparation steps, number of ingredients, and tags that help categorize the recipe by cuisine and style. Through our analysis, we aim to reveal trends and preferences in recipe ratings, offering insights into what makes a recipe popular among users, with a special focus on protein, sugar, and number of ingredients."


## Data Cleaning and Exploratory Data Analysis

### Data cleaning
In the analytical section of our report, we meticulously merged and cleaned the recipe data to prepare it for a comprehensive analysis of how sugar and protein content influence recipe ratings. Initially, we combined the recipes dataset with the ratings dataset using the `recipe_id` as the key to link each recipe to its corresponding reviews and ratings. This allowed us to align the nutritional information directly with user feedback.

Following the merge, our focus shifted to extracting crucial nutritional components—sugar and protein—from the `nutrition` list embedded within each recipe record. The `nutrition` attribute contained a list of values, where sugar and protein were positioned as the third and fifth elements respectively. 

Utilizing Python’s list indexing, we extracted these elements to form new, dedicated columns for each nutritional metric. This data extraction was crucial as it enabled us to specifically analyze the impact of these nutrients on the recipes' ratings.

There is an overview of the dataset after cleaning
| user_id | recipe_id | rating | nutrition                                     | n_ingredients | sugar | protein |
|---------|-----------|--------|-----------------------------------------------|---------------|-------|---------|
| 483827  | 306785    | 5      | [95.3, 1.0, 50.0, 16.0, 5.0, 0.0, 7.0]       | 8             | 50.0  | 5.0     |
| 5060    | 310237    | 5      | [143.5, 5.0, 25.0, 3.0, 10.0, 3.0, 7.0]      | 10            | 25.0  | 10.0    |
| 935485  | 321038    | 5      | [182.4, 2.0, 50.0, 7.0, 11.0, 1.0, 13.0]     | 14            | 50.0  | 11.0    |
| 539686  | 321038    | 5      | [182.4, 2.0, 50.0, 7.0, 11.0, 1.0, 13.0]     | 14            | 50.0  | 11.0    |
| 22174   | 342209    | 4      | [658.2, 45.0, 151.0, 35.0, 24.0, 72.0, 29.0] | 12            | 151.0 | 24.0    |



### Univariate Analysis

To ensure the quality and reliability of our data, we conducted a preliminary analysis using a box plot to visualize the distribution of sugar content across recipes. The original visualization revealed a wide range of values, with some extreme outliers stretching far beyond typical culinary sugar amounts. In response, we adjusted the plot to focus on a more typical range, setting the x-axis limit to 300 grams. This adjustment made it apparent that the majority of sugar values clustered well below this threshold, indicating that values above 300 grams were rare and not representative of typical recipes

<iframe src="./imgs/sugar_box_before.html" width="90%" height="600px" frameBorder=0></iframe>



From this refined box plot, we concluded that recipes containing sugar amounts within the observed range (0 to just below 300 grams) were most relevant for further analysis, while those exceeding 300 grams were considered outliers and excluded from predictive modeling. This decision was based on the rarity of such high sugar values and aimed at enhancing the accuracy and applicability of our predictive insights, focusing on dietary habits that are more common and representative of typical consumer choices. By maintaining this focus, we ensure that our findings remain relevant and practical for typical culinary applications and dietary considerations.

<iframe src="./imgs/sugar_box_after.html" width="90%" height="600px" frameBorder=0></iframe>

Similar to our approach with sugar, we analyzed the distribution of protein content in recipes, setting a threshold at 200 grams to exclude outliers. This decision ensures our analysis remains focused on typical dietary recipes, excluding extreme high-protein dishes that are not standard in everyday meals. Our goal is to provide insights that are applicable to the average consumer, enhancing the practical value of our findings.

<iframe src="./imgs/protein_box_before.html" width="90%" height="600px" frameBorder=0></iframe>

<iframe src="./imgs/protein_box_after.html" width="90%" height="600px" frameBorder=0></iframe>

### Bivariate Analysis

#### Interpreting the Scatter Plot For Sugar and Rating

**Distribution Trends**: The scatter plot reveals that as sugar content varies, the ratings mostly cluster between 4.2 and 4.6. There doesn't appear to be a clear trend or pattern indicating a strong linear relationship between sugar content and average ratings, as the data points are quite spread out horizontally.

**Potential Correlation**: The lack of a visible upward or downward trend suggests that there may not be a strong direct correlation between sugar content and recipe ratings. However, this does not rule out other forms of relationship or underlying factors that could influence these variables.

**Outlier Consideration**: Even with the sugar cutoff at 300 grams, the plot shows some spread in ratings at various sugar levels, but there are no extreme outliers in sugar content that distort the analysis. This supports your decision to limit sugar content to 300 grams in your analysis.

<iframe src="./imgs/sugar_rating_scatter.html" width="90%" height="600px" frameBorder=0></iframe>

#### Interpreting the Scatter Plot For Protein and Rating

<iframe src="./imgs/protein_rating_scatter.html" width="90%" height="600px" frameBorder=0></iframe>

#### Interesting Aggregation
### What We Did:
- We divided recipe ratings into three categories:
  - Low Ratings: Ratings ≤ 3.5
  - Medium Ratings: Ratings between 3.5 and 4.5
  - High Ratings: Ratings > 4.5
- Grouped and Aggregated Data:
    - We grouped the data by these rating categories and calculated the average sugar and protein content for each group. This created a pivot table showing the average nutritional content for each rating category.
- Interpreted the Results:
    - Using the pivot table, we analyzed trends in sugar and protein content across the three rating categories.

| rating_category   |   protein |   sugar |
|:------------------|----------:|--------:|
| High              |   31.1183 | 43.3707 |
| Low               |   32.5417 | 45.8386 |
| Medium            |   34.0743 | 40.8115 |

### What We Found:
- Sugar Content
  - Recipes with low ratings had the highest average sugar content (45.17 grams).
  - Recipes with medium ratings had least sugar on average (38.88 grams).
  - Recipes with high ratings had the slightly lower average sugar content (41.54 grams).
  
**Key Insight**: Higher sugar content may negatively influence ratings. Users might prefer recipes with less sugar, potentially because they are perceived as healthier or more balanced in flavor.
 - Protein Content:

    The average protein content was fairly consistent across categories:
    - Low Ratings: 34.25 grams
    - Medium Ratings: 33.47 grams
    - High Ratings: 31.84 grams

**Key Insight**: Protein content does not appear to have a significant impact on recipe ratings. The variation in protein levels across rating categories is minimal.

## Assessment of Missingness

We begin by identifying which columns in the dataset contain missing values. Note that that the missing values are not a result of the merging process, as we previously used an inner join to combine the `recipes` and `interaction` dataframes

|                |   Num of Missing Value |
|:---------------|----:|
| user_id        |   0 |
| recipe_id      |   0 |
| date           |   0 |
| rating         |   0 |
| review         |  55 |
| name           |   1 |
| id             |   0 |
| minutes        |   0 |
| contributor_id |   0 |
| submitted      |   0 |
| tags           |   0 |
| nutrition      |   0 |
| n_steps        |   0 |
| steps          |   0 |
| description    | 113 |
| ingredients    |   0 |
| n_ingredients  |   0 |

### NMAR Analysis:
 During our analysis, we identified that some recipes lack reviews and we believe that will be NMAR. Our investigation suggests that this missingness could potentially relate to the recipes' complexity or popularity, indicating that it might not be random. For instance, users might choose not to leave a review for recipes they didn’t enjoy, as negative experiences could discourage them from engaging further with the platform. Similarly, recipes that are particularly complex, requiring many steps or unusual ingredients, might also go unreviewed. This could happen because users may abandon these recipes partway through or feel that their feedback wouldn’t be constructive. Additionally, some users might avoid leaving reviews for recipes they didn’t personally prepare but simply viewed, resulting in gaps in the data tied to these specific scenarios.

### Missingness Dependency:
We run 1000 permutation test on the column `n_steps` based on the following Hypothesis:

- **Null Hypothesis**: The distribution of `n_steps` is the same for recipes with missing review and without missing review 
- **Alternative Hypothesis**: The distribution of `n_steps` is different between recipes with missing review and without missing review, thus `review` is NMAR depends on `n_steps`

And we use **Absolute Difference in Mean** as our test statistics

<iframe src="./imgs/missingness.html" width="90%" height="600px" frameBorder=0></iframe>

From both plot intepretation and numerical calculation

We get **P-value** of 0.0 for our permuation test. This means we reject the null hypothesis that the distribution of `n_steps` is the same for recipes with missing review and without missing review.


### Hypothesis Testing: 
**Objective**: From the scatter plot between sugar content and rating, delves into how the sugar content of recipes influences the diversity of user ratings. Understanding this variance helps us gauge consumer preferences and dietary perceptions related to sugar.

<iframe src="./imgs/sugar_rating_scatter.html" width="90%" height="600px" frameBorder=0></iframe>

**Null Hypothesis ($H_0$)**
The null hypothesis states that there is no difference in the variance of recipe ratings between high-sugar recipes (more than 150 grams of sugar) and low-sugar recipes (150 grams of sugar or less). This implies that the sugar content does not affect the variability in how recipes are rated.


$H_0: \text{Var}(\text{ratings}|\text{sugar} >150) = \text{Var}(\text{ratings}|\text{sugar} \leq 150)$

**Alternative Hypothesis ($H_\alpha$)**
The alternative hypothesis contends that there is a difference in the variance of ratings between high-sugar and low-sugar recipes. This suggests that the amount of sugar in a recipe does influence the variability in ratings, possibly due to varying preferences or perceptions among users about sugar content.


$H_\alpha: \text{Var}(\text{ratings}|\text{sugar} >150) \neq \text{Var}(\text{ratings}|\text{sugar} \leq 150)$

##### Test statistic
Absolute difference in Variances 

$\text{Test Statistic} = \left|\text{Var}(\text{high sugar}) - \text{Var}(\text{low sugar})\right|$

We ran 1000 permutation test on the `rating` column and get the following result

<iframe src="./imgs/hypo_test.html" width="90%" height="600px" frameBorder=0></iframe>

**P_value = 0**: The result strongly suggests that the amount of sugar in recipes has a significant effect on the variance of the ratings they receive. This could be interpreted to mean that sugar content impacts user satisfaction or opinion diversity.


Since the p-value is 0, we reject the null hypothesis. This conclusion indicates that the difference in variance between high sugar and low sugar recipes is statistically significant and not due to random chance.
Implications: Rejecting the null hypothesis suggests that sugar content does significantly affect the variance in ratings. Recipes with higher sugar might be causing more diverse reactions among the users, possibly due to varying preferences or health considerations.



#### Framing the Problem ####

In our ongoing exploration of what influences recipe ratings on our platform, we are now developing a model to predict the ratings a recipe might receive based on various attributes. This predictive insight can empower recipe creators to adjust their recipes in ways that enhance user satisfaction.

We aim to predict the ratings recipes receive based on three key features: sugar content, number of preparation steps, and total preparation time. These factors have been chosen based on their potential impact on user preferences and previous analytical findings showing sugar's significant role in rating variability.

### Baseline Model

In our quest to understand what influences a recipe's rating, we've established a baseline model that predicts ratings based on three key attributes: the sugar content, protein content, and number of ingredients in a recipe. This initial model sets the groundwork for our predictive analytics, allowing us to gauge feature significance and model effectiveness right from the start.

#### Model Structure: 
Our baseline model incorporates a linear regression algorithm, chosen for its simplicity and interpretability. To prepare our data for modeling, we employed a combination of standard scaling for sugar and protein levels to normalize these features and a quantile transformation for the number of ingredients to reduce the impact of outliers and skewness in the data distribution.

|       |    RMSE |
|:------|--------:|
| Train | 1.076148| 
| Test  | 1.087220| 

Our baseline model achieved an RMSE of 1.076148 on the training set and 1.087220 on the testing set. These initial results are promising as they indicate the model's ability to predict ratings with reasonable accuracy, serving as a benchmark for further enhancements. While our baseline model provides a good starting point, we plan to explore more complex models and additional features that may capture the nuances of recipe ratings more effectively.

### Final Model

#### Introduction to Final Model: 
In our pursuit to refine our predictive model and improve upon our baseline predictions, we introduced additional features and employed a more complex modeling technique. Our final model leverages a Random Forest Regressor, known for its robustness and ability to handle non-linear relationships, to predict recipe ratings.

#### Feature Selection and Engineering:
- **Sugar Content**: Continued from our baseline model, sugar content is standardized to account for scale differences and its impact on taste preferences is further analyzed.
- **Protein Content**: Also standardized, we explore protein's role in determining how health-conscious decisions influence recipe ratings.
- **Number of Ingredients**: We quantify complexity and use a quantile transformation to normalize this feature, reducing skewness.
- **Interaction Terms**: We introduced interaction terms between sugar and protein to capture their combined effects on ratings.
- **Polynomial Features**: To better capture non-linear relationships, polynomial features for sugar and protein were added.

### Model Enhancements:
 - **Random Forest Regressor**: We chose this model for its effectiveness in handling various types of data and its capability to improve generalization over the baseline model.
- **Hyperparameter Tuning**: Utilizing GridSearchCV, we meticulously searched for the best hyperparameters, including max_depth, n_estimators, min_samples_split, and max_features, to optimize our model's performance.

### Performance Evaluation:
- **Metric Used**: We evaluated our model using RMSE to measure the average magnitude of the prediction errors, providing us with a clear indicator of model accuracy.
- **Results**: Our final model achieved an RMSE of 1.0814234983971323, a noticeable improvement from the baseline model with 0.056. This indicates a more accurate model that better captures the underlying patterns in the data.

