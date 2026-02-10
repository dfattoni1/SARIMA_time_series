# Customer Segmentation: Unsupervised Learning

**Creating customer segments for a subset of OkCUpid users based on their characteristics through an unsupervised machine learning model.**

## Project overview
Dating sites try to pair users to create the best potential matches, so having a way to determine whether two users are more likely to share certain characteristics or not is a great first step in developing better recommendation systems for consumers. With this in mind, the purpose of this project was to build clusters of people based on shared characteristics so as to get a better understanding of how the consumer base is distributed and how to start making recommendations on potential matches based on shared traits.

## The data
The data for this Project was obtained through a publicly available Kaggle dataset on OkCupid profiles. The dataset can be accessed [here](https://www.kaggle.com/datasets/andrewmvd/okcupid-profiles). All the data is contained within a single file, profiles.zip (the data is compressed in order to meet GitHub's file size requirements), which can be found in the data folder of this project. The dataset includes 31 columns, though only 20 were used for the analysis.

## Project development
This section will explain the process of data preprocessing, model selection, and hyperparameter tuning. Also, key insights relating to the findings of the analysis will be included.

### Data preprocessing
Despite the dataset containing 31 columns originally, some of them were discarded from the analysis due to either extremely high cardinality making the interpretation of the results unviable or simply due to practically non-existent variability. Also, most of the columns in the dataset were categorical, which also presented an additional challenge in terms of preprocessing and encoding. Certain columns also contained multiple pieces of information at once, which made it necessary to split them into more than one column. Below are some examples of this:

| Feature | Example row | Split into |
| :--- | :--- | :--- |
| diet | strictly vegetarian | diet_strictness and diet_type |
| offspring |  don't have kids, but wants them | has_kids and wants_kids |
| religion | agnosticism and very serious about it | religion_type and religion_importance |

Lastly, in features in which there were missing values, imputation was carried out as long as it was sensible to do so. Features such as income in which more than 50% of users decided not to disclose it, performing any type of imputation could result in extremely misleading results, so the feature was simply recoded to show whether the user disclosed their income or not. In other situations such as with age, other features could be used to estimate what the missing values could be. For instance, it was possible to leverage the fact that we know a user is a smoker, lives in California, and is enrolled in a master's degree program to get an estimation of how old they are. If they're smokers in California, they must be at least 21 years old. Also, knowing they're a master's student, it's possible to get the average age of other users who are also enrolled in a master's degree and impute the missing value.

### Model selection and hyperparameter tuning
The 20 features included were a mix of primarily categorical and also some quantitate variables, so a K-Prototypes Clustering algorithm was chosen for the customer segmentation, as this allows for clustering without having to give up categorical or quantitative variables. Using the cost as a metric, it was possible to determine that 4 clusters was the most appropriate amount to segregate users using the elbow method. This was later corroborated visually by plotting the cost at different values of K.

![Cost at different values of K](images/cost_by_k.png)

*Figure 1: Cost at different values of K showing 4 is where it starts dropping more gradually.*

Having found the optimal number of clusters, gamma was tuned to account for the imbalance in categorical and quantitative features. With the full model, 4 distinct clusters were identified and the characteristics that made them stand out from the rest were highlighted.

### Key insights
Four clusters were identified:

_**The private neutrals:**_
Represent the majority of the user base. They prefer not to disclose too much information about themselves.

_**The casual urbanites:**_
They have an average body type, eat anything, work in Business/Finance, and have a casual relationship with beliefs.

_**The compassionate educators:**_
the unique female majority and their professional focus on Health and Education. "Compassionate" leans into the dog-lover and "helper" industry traits.

_**The mature traditionalists:**_
They are the oldest group, have a religious affiliation (Catholic), and a established career path. They are a niche but highly defined demographic.

The full analysis, including the evaluation of the model, can be found [here](date-a-scientist.ipynb).

## How to explore this repository
**Analysis:** View the full the analysis in the [Jupyter Notebook](date-a-scientist.ipynb) for full EDA and modelling.

**Utilities:** Custom functions for plotting are in [util.py](util.py).

**Reproduction:** Run `pip install -r requirements.txt` to set up the environment.