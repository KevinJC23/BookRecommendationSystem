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
- Evaluating and choosing a recommendation system algorithm that provides accurace and highly relevant book suggestions.

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
### Sampling to Reduce Dataset Size
Due to an overwhelming amount of rows —approximately 3,000,000— and limited resource to manage the data. We extracted 25,000 random sample entries to simplify the dataset numbers. This sampling was performed using ```.sample()``` methods by ```n=25000``` and ```random_state=42```, where it ensures reproducibility of results. After sampling, index was reset with ```drop=True``` to maintain a clean index in the new subset.

### Selecting Relevant Columns
Select the desired columns such as ```Id```, ```Title```, ```User_id```, and ```review/score```, and assign them into ```cf_books``` variable. Additionally, any rows containing missing values were dropped using  ```.dropna()``` methods to ensure the dataset is complete and clean for modeling.

### Dropping Duplicate Records
Entries containing duplicate values—identified during the initial data inspection—were eliminated to ensure data integrity and prevent redundancy. Although only four duplicate records were found, their removal contributes to maintaining a clean and unbiased dataset.

### Sorting Title Column
Create a variable named `cf_books_sort` to store the dataset sorted alphabetically by the Title column.

### TF-IDF For Content Based Filtering
Items are typically described through a set of features derived from their metadata or textual content—such as genre, keywords, or descriptions. One common approach is to apply a TF-IDF vectorizer to extract important feature representations from each book title, then fit and transform the text into numerical matrices. Since the output of the TF-IDF vectorizer is a sparse matrix, it is often converted into a dense format using .dense() for further processing. This transformation is part of the data preparation stage, where raw item information is cleaned, structured, and vectorized to be suitable for modeling.

### Encoding And Normalizing Data For Collaborative Filtering
To prepare the dataset for collaborative filtering, UserID and Title were encoded into numeric values using a mapping dictionary. This encoding process allows the data to used effectively in an embedding layer in model architecture, that will learn latent representation of UserID and title. Review scores were normalized to a 0-1 scale to match the expected output range of sigmoid function that is used during model training. Finally, dataset was randomly shuffled and divided into training set (80%) and validation set (20%), to ensure a balance and unbiased model evaluation and to reduce overfitting risks.

## Modeling
### Content Based Filtering
In content-based filtering, modeling refers to the process of constructing abstract representations of items and users based on their intrinsic characteristics. Users are modeled by aggregating the attributes of items they have previously interacted with, thereby forming a personalized preference profile. This dual representation (user profile and item feature vectors) enables the system to compute similarity scores—commonly through cosine similarity—between a user profile and the feature vectors of unseen items. The modeling phase is essential for capturing latent patterns in content and user preferences, ultimately allowing the recommender system to suggest items that align closely with an individual’s historical interests. This approach emphasizes the relevance of item features over user interactions, making it particularly effective in scenarios with sparse collaborative data. The content-based filtering approach has the advantage of being resilient to the cold-start problem for new users and is effective in domains with rich metadata. However, it has limitations in capturing collective preferences or general trends among users.

Here is the result of Collaborative Filtering:

Books title: Buddha Mom: The Journey Through Mindful Mothering

Most similar book:
| Title                                                | Id          |
|------------------------------------------------------|-------------|
| My Mom Is a Dragon                                   | 0971594058  |
| The Buddha in the Robot                              | B0006EAV8Q  |
| The Buddha of Suburbia                               | 014013168X  |
| In the Buddha's Words: An Anthology of Discourses    | 0861714911  |
| Such a Long Journey                                  | 0864922191  |

This result returns the top 5 books that are most similar to "Buddha Mom: The Journey Through Mindful Mothering" in terms of their TF-IDF feature representation using Cosine Similarity.

### Collaborative Filtering
The collaborative filtering model employed in this recommendation system is implemented using a neural network architecture grounded in latent factor modeling via embedding layers. The model, named RecommenderNet, is designed to learn implicit patterns in user-item interactions by mapping both users and items (book titles) into dense vector spaces of fixed dimensionality (in this case, 50). Each user and item is assigned an embedding vector that captures latent features representing preferences and characteristics which are not explicitly observable in the raw data. Additionally, user-specific and item-specific bias terms are incorporated to account for individual tendencies in rating behavior. 

The core of the model computes the dot product between the user and item embeddings to estimate the degree of affinity between them. This result is augmented by the respective bias terms and passed through a sigmoid activation function to constrain the predicted score to a normalized range between 0 and 1. The model is trained using the binary cross-entropy loss function, optimized via the Adam algorithm, and evaluated using Root Mean Squared Error (RMSE) as a performance metric. To enhance generalization and prevent overfitting, early stopping is employed based on the validation RMSE, halting training once performance ceases to improve over a set number of epochs. This embedding-based collaborative filtering approach effectively models personalized recommendations without requiring any content-based features, making it particularly well-suited for large-scale systems where explicit metadata may be sparse or unavailable.

The collaborative filtering excels at detecting implicit patterns and providing personalized recommendations without requiring content features. However, this model heavily relies on the quantity and quality of historical interactions; in cases of highly sparse data or new users/items, its performance can degrade significantly.

Here is the result of Collaborative Filtering:

User A1ICSOSXOBZENW Has Rated the Following Books:
| Title             | Review/Score |
|-------------------|--------------|
| Spirit of Family  | 5.0          |

Top Book Recommendation For A1ICSOSXOBZENW:
| Title                                                                 | Review/Score |
|-----------------------------------------------------------------------|--------------|
| A CHRISTMAS CAROL, BEING A GHOST STORY OF CHRISTMAS                   | 5.0          |
| The richest man in Babylon                                            | 5.0          |
| Taking Charge of Your Fertility: The Definitive Guide...              | 5.0          |
| Seabiscuit: An American Legend (Trade Edition)                        | 5.0          |
| Redeeming Love                                                        | 5.0          |
| THE BOOK OF MORMON: Another Testament of Jesus Christ                 | 5.0          |
| The China Study: The Most Comprehensive Study...                      | 5.0          |
| The Count Of Monte Cristo, Abridged                                   | 5.0          |
| With the Old Breed: At Peleliu and Okinawa                            | 5.0          |
| Wizard's First Rule (Sword of Truth, Book 1)                          | 5.0          |
| Seabiscuit: An American Legend (Trade Edition)                        | 2.0          |
| Wizard's First Rule (Sword of Truth, Book 1)                          | 4.0          |
| With the Old Breed: At Peleliu and Okinawa                            | 3.0          |
| Taking Charge of Your Fertility: The Definitive Guide...              | 2.0          |
| The richest man in Babylon                                            | 4.0          |
| Redeeming Love                                                        | 2.0          |
| Wizard's First Rule (Sword of Truth, Book 1)                          | 1.0          |

Based on the result, the system recommend user's by book title such as "A Christmas Carol" and "The Richest Man in Babylon", each with high predicted interest. Despite variations in actual dataset ratings, these books were ranked highly by model due to their relevance to user's profile and reading history.

## Evaluation
### Content Based Filtering
To evaluate performance of the Content Based Filtering recommendation system performance, we tried to use ```Buddha Mom: The Journey Through Mindful Mothering``` book as the query item. this system was expected to retrieve the relevant items such as ```My Mom Is a Dragon```, ```The Buddha in the Robot```, and ```The Buddha of Suburbia```. This result shows recommendation system works effectively, especially because successfully took all relevant items in top 5 recommendations with ```Recall@5 = 1.0``` and rank at least one relevant item in the top position with ```Mean Reciprocal Rank (MRR) = 1.0```. Although ```Precision@5 = 0.6``` is slightly below perfect —indicating that 3 out of 5 recommended items were not relevant— system still shows good performance overall by balancing accuracy and ranking quality, as also reflected in ```F1@5: 0.7499999999999999```.

### Collaborative Filtering
#### RMSE (Root Mean Square Error)
Root Mean Squared Error (RMSE) is an evaluation metric that is used to measure difference between model prediction and actual values. RMSE calculated  as the square root of the average of the squared prediction errors. A lower RSME value shows better predictive model performance.

![download](https://github.com/user-attachments/assets/00de995d-f3e7-4e52-8ab2-af5eb76014c9)

This graph indicates that the model performs increasingly well on the training data, while its performance on the validation data remains relatively constant. This suggests a risk of overfitting, as the model fails to generalize to unseen data. This observation is supported by the final training and validation metrics, where the model achieved a loss of 0.2074 and root mean squared error (RMSE) of 0.0243 on the training set, but a significantly higher validation loss of 0.6547 and validation RMSE of 0.3840.

## Conclusion
Based on the evaluation conducted, Content-Based Filtering has proven to be the most suitable model to implement in this book recommendation system. From a business understanding perspective, this model provides a significant positive impact by delivering recommendations that are relevant and aligned with user preferences, which in turn has the potential to enhance user satisfaction and engagement on the platform. All problem statements that were previously defined have been successfully addressed. The system is capable of generating personalized recommendations, applying algorithms effectively to improve user experience, and identifying the most accurate method for delivering relevant suggestions. The goals set for the system development were also achieved, particularly in utilizing user review data to provide meaningful recommendations and increasing satisfaction through more personalized results.

In addition, the solution strategy of comparing two recommendation algorithms has proven to be impactful. Through the comparison between Content-Based Filtering and Collaborative Filtering, a clear understanding was gained regarding the effectiveness of each approach in terms of accuracy and relevance. The results indicate that the content-based approach is more stable and reliable, especially in situations where user data is limited or incomplete. Therefore, Content-Based Filtering can serve as a strong foundation for future recommendation system development, either as the primary model or as a component in a more complex hybrid system, supporting long-term business strategy success and enhancing user experience.

## References
Kushmeeta, C., & Rout, S. K. (2013). Reading Habits - An Overview. IOSR Journal of Humanities and Social Science, 14(6), 14–15. http://www.iosrjournals.org/iosr-jhss/papers/Vol14-issue6/C01461317.pdf

OECD. (2023). PISA 2022 Results (Volume I and II): Country Notes: Indonesia. Organisation for Economic Co-operation and Development. https://www.oecd.org/en/publications/pisa-2022-results-volume-i-and-ii-country-notes_ed6fbcc5-en/indonesia_c2e1ae0e-en.html
