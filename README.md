# Machine Learning Project Report - Kevin Juan Carlos

## Project Overview
Reading books offers numerous benefits for both individual and societal development, like increasing literacy skills, expand their knowledge, strengthening critical thinking, also fostering character and social empathy. According to Kushmeeta dan Rout (2013), cultivating strong reading habits since an early age is very important, because it can significantly impact children's mental and academic achievement. Children who read books for their pleasure unconsciously increase their reading proficiency and language skills (Reyhene, 1998; Bignold, 2003). However, literacy levels in Indonesia remains a cause of concern.

Based on PISA 2022 results which were released by OECD, around 25% of students in Indonesia achieve Level 2 or higher in reading—significantly below OECD average around 74%. Level 2 shows fundamental skills like identifying main idea in a text and finding information based on explicit criteria. Even more concerning is the near absence Indonesian students who achieve Level 5 or higher, which reflect skills for understanding long and complex texts also differentiate facts from opinions using explicit clues. This low literacy rate has a big impact on human resources and nation competitiveness. Literacy is cornerstone for educational advancement, productivity, and active participation in society. Without strong reading culture, individual will have difficulties to adapt to demands of the digital globalization era.

One viable solution to address this issue is book recommendation system development. This kind of system can assist the reader in discovering a book tailored to their interests, age, and preferences, thereby fostering strong engagement in reading activity. With this personal and adaptive approach, this system has potential to increase reading motivation and create sustainable reading habits.

## Business Understanding
### Problem Statements
- How to produce book recommendations that can be personalized based on each user's preferences?
- How can recommendation system algorithm be effectively implemented to increase user satisfaction and engagement?
- What recommendation system algorithm can give accurate and relevant results for book suggestions?

### Goals
- Develop a recommendation system that leverage user review scores to suggest books aligned with user's preferences.
- Increase user satisfaction by delivering relevant and personalized book recommendations.

### Solution Statements
- Compare two recommendation algorithms to decide which model is the most effective in giving accurate recommendations by comparing Content-Based Filtering—precision, recall, F1, MMR (Mean Reciprocal Rank)— and Collaborative Filtering—RMSE (Root Mean Square Error)— metrics.

## Data Understanding
### Source
The dataset used in this project was obtained from [Kaggle](https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews) with the name "Amazon Books Reviews". It can be downloaded using kagglehub library with the following code:
```
import kagglehub

path = kagglehub.dataset_download("mohamedbakhet/amazon-books-reviews")
```

### Dataset Information
This dataset originally contains ten columns and consists of 3,000,000 entries before data cleaning. Due to resource limitations, it was reduced into 25,000. 

### Data Condition
- There are no missing values.
- Contains 4 duplicate entries.
- Title column consist of 12,551 unique book titles.

The dataset consists of ten columns, which are as follows:
- Id: Unique identifier for book.
- Title: Title of the book.
- Price: Price of the book.
- User_id: Unique identifier for user who rate the book.
- profileName: Name of user who rate the book.
- review/helpfulness: Helpfulness score of the review.
- review/score: The rating given to the book from 0 to 5.
- review/time: Time when the user submitted a book review.
- review/summary: Brief summary of text review.
- review/text: Full written content of the review.

In this project, we use only four columns such as Id, Title, User_id, review/score which are related to the goal of building recommender system.

### Datatypes Overview
This part explained datatypes from each column on dataset. Datatype shows how data stored and processed, example is the data in number (integer, float), text (object), or categorical and also this is become basic foundation to create a better recommendation system. 
| Column        | Non-Null Count | Dtype   |
|---------------|----------------|---------|
| Id            | 20,385         | object  |
| Title         | 20,385         | object  |
| User_id       | 20,385         | object  |
| review/score  | 20,385         | float64 |

### Datasets Descriptive Statistic
This part displays the statistical summary of column on the dataset's numeric features.
| Statistic      | review/score |
|----------------|--------------|
| count          | 20,385       |
| mean           | 4.234        |
| std            | 1.176        |
| min            | 1.000        |
| 25%            | 4.000        |
| 50%            | 5.000        |
| 75%            | 5.000        |
| Max            | 5.000        |

### Data Distribution Visualization
This part presenting visualization of numeric feature.

- #### Boxplot for review/score Distribution
![review/score](https://github.com/user-attachments/assets/bd77b2e3-183a-4836-9b6b-c6bfa5c47ac8)

Based on the boxplot, distribution of review/score is positively skewed (right-skewed). However, since the data will be used to generate recommendations that prioritize higher ratings, removing outlier may not be necessary.

## Data Preparation
### Dropping Duplicate Records
Entries containing duplicate values—identified during the initial data inspection—were eliminated to ensure data integrity and prevent redundancy. Although only four duplicate records were found, their removal contributes to maintaining a clean and unbiased dataset.

### Sorting Title Column
Create a variable named `cf_books_sort` to store the dataset sorted alphabetically by the Title column.

### Encoding And Normalizing Data For Collaborative Filtering
To prepare the dataset for collaborative filtering, UserID and Title were encoded into numeric values using a mapping dictionary. This encoding process allows the data to used effectively in an embedding layer in model architecture, that will learn latent representation of UserID and title. Review scores were normalized to a 0-1 scale to match the expected output range of sigmoid function that is used during model training. Finally, dataset was randomly shuffled and divided into training set (80%) and validation set (20%), to ensure a balance and unbiased model evaluation and to reduce overfitting risks.

## Modeling
### Content Based Filtering
In content-based filtering, modeling refers to the process of constructing abstract representations of items and users based on their intrinsic characteristics. Items are typically described through a set of features derived from their metadata or textual content—such as genre, keywords, or descriptions—which are often transformed into numerical vectors using techniques like TF-IDF (Term Frequency–Inverse Document Frequency). Users, on the other hand, are modeled by aggregating the attributes of items they have previously interacted with, thereby forming a personalized preference profile. This dual representation enables the system to compute similarity scores—commonly through cosine similarity—between a user profile and the feature vectors of unseen items. The modeling phase is essential for capturing latent patterns in content and user preferences, ultimately allowing the recommender system to suggest items that align closely with an individual’s historical interests. This approach emphasizes the relevance of item features over user interactions, making it particularly effective in scenarios with sparse collaborative data.

### Collaborative Filtering
The collaborative filtering model employed in this recommendation system is implemented using a neural network architecture grounded in latent factor modeling via embedding layers. The model, named RecommenderNet, is designed to learn implicit patterns in user-item interactions by mapping both users and items (book titles) into dense vector spaces of fixed dimensionality (in this case, 50). Each user and item is assigned an embedding vector that captures latent features representing preferences and characteristics which are not explicitly observable in the raw data. Additionally, user-specific and item-specific bias terms are incorporated to account for individual tendencies in rating behavior.

The core of the model computes the dot product between the user and item embeddings to estimate the degree of affinity between them. This result is augmented by the respective bias terms and passed through a sigmoid activation function to constrain the predicted score to a normalized range between 0 and 1. The model is trained using the binary cross-entropy loss function, optimized via the Adam algorithm, and evaluated using Root Mean Squared Error (RMSE) as a performance metric.

To enhance generalization and prevent overfitting, early stopping is employed based on the validation RMSE, halting training once performance ceases to improve over a set number of epochs. This embedding-based collaborative filtering approach effectively models personalized recommendations without requiring any content-based features, making it particularly well-suited for large-scale systems where explicit metadata may be sparse or unavailable.

## Evaluation
### Content Based Filtering
To evaluate performance of the Content Based Filtering recommendation system performance, we tried to use "Buddha Mom: The Journey Through Mindful Mothering" book as the query item. this system was expected to retrieve the relevant items such as ```My Mom Is a Dragon```, ```The Buddha in the Robot```, and ```The Buddha of Suburbia```. This result shows recommendation system works effectively, especially because successfully took all relevant items in top 5 recommendations with ```Recall@5 = 1.0``` and rank at least one relevant item in the top position with ```Mean Reciprocal Rank (MRR) = 1.0```. Although ```Precision@5 = 0.6``` is slightly below perfect —indicating that 3 out of 5 recommended items were not relevant— system still shows good performance overall by balancing accuracy and ranking quality, as also reflected in ```F1@5: 0.7499999999999999```.

### Collaborative Filtering
#### RMSE (Root Mean Square Error)
Root Mean Squared Error (RMSE) is an evaluation metric that is used to measure difference between model prediction and actual values. RMSE calculated  as the square root of the average of the squared prediction errors. A lower RSME value shows better predictive model performance.

![download](https://github.com/user-attachments/assets/00de995d-f3e7-4e52-8ab2-af5eb76014c9)

This graph indicates that the model performs increasingly well on the training data, while its performance on the validation data remains relatively constant. This suggests a risk of overfitting, as the model fails to generalize to unseen data. This observation is supported by the final training and validation metrics, where the model achieved a loss of 0.2074 and root mean squared error (RMSE) of 0.0243 on the training set, but a significantly higher validation loss of 0.6547 and validation RMSE of 0.3840.

## Conclusion
All problem statements outlined in the business understanding phase have been satisfactorily addressed. The system successfully generates personalized book recommendations aligned with individual user preferences, demonstrating a tangible impact on user satisfaction and engagement. The deployment of recommendation algorithms has yielded relevant and targeted suggestions, and the comparative evaluation of multiple approaches has allowed for the identification of the most effective model, thereby meeting the third objective.

In summary, the implemented solutions have significantly contributed to achieving the overarching business goals. Nonetheless, there remains scope for refinement, particularly in enhancing the generalizability of the collaborative filtering model. Future improvements could involve regularization techniques, hyperparameter optimization, or hybrid modeling strategies to mitigate overfitting and further improve system robustness in real-world applications.


## References
Kushmeeta, C., & Rout, S. K. (2013). Reading Habits - An Overview. IOSR Journal of Humanities and Social Science, 14(6), 14–15. http://www.iosrjournals.org/iosr-jhss/papers/Vol14-issue6/C01461317.pdf

OECD. (2023). PISA 2022 Results (Volume I and II): Country Notes: Indonesia. Organisation for Economic Co-operation and Development. https://www.oecd.org/en/publications/pisa-2022-results-volume-i-and-ii-country-notes_ed6fbcc5-en/indonesia_c2e1ae0e-en.html
