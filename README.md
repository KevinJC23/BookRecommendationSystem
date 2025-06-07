# Machine Learning Project Report - Kevin Juan Carlos

## Project Overview
Reading books offers numerous benefits for both individual and societal development, such as enhancing literacy skills, broadening knowledge, strengthing critical thinking, and fostering character and social empathy. According to Kushmeeta dan Rout (2013), cultivating strong reading habits from an early age is essential, as it significantly impacts children's mental growth and academic success. Children who read for pleasure unconsciously improve their reading proficiency and language skills (Reyhene, 1998; Bignold, 2003). However, Reading literacy level in Indonesia remains a cause for concern.

Based on the 2022 PISA results released by the OECD, only about 25% of Indonesian students achieved Level 2 or higher in reading—significantly below the OECD average of 74%. Level 2 denotes fundamental skills such as identifying the main idea in a text and locating information based on explicit criteria. Even more concerning is the near absence of Indonesian students reaching Level 5 or above, which reflects the ability to comprehend lengthy and complex texts and to distinguish fact from opinion using implicit cues. This low literacy rate has a profound impact on the quality of human resources and the nation’s competitiveness. Literacy serves as the cornerstone for educational advancement, productivity, and active participation in society. Without a strong reading culture, individuals will struggle to adapt to the demands of the digital age and globalization.

One viable solution to address this issue is the development of a book recommendation system. Such a system can assist readers in discovering books tailored to their interests, age, and preferences, thereby fostering greater engagement with reading activities. Through a personalized and adaptive approach, this system holds the potential to boost reading motivation and cultivate sustainable reading habits.

## Business Understanding
### Problem Statements
- How can we generate personalized book recommendations based on individual user preferences?
- How can recommendation system algorithms be effectively applied to improve user satisfaction and engagement?
- Which recommendation system algorithm provides the most accurate and relevant results for book suggestions?

### Goals
- To develop a recommender system that leverages user review scores to suggest books aligned with user preferences.
- To enhance user satisfaction by delivering relevant and personalized book recommendations.
- To evaluate and identify the most effective recommendation algorithm for this dataset using performance metrics such as precision, recall, or RMSE.

### Solution Statements
- Use collaborative filtering techniques (e.g., user-based or item-based) to build a recommendation engine based on historical review scores.
- Train and evaluate multiple recommendation algorithms to determine the most effective model for delivering accurate recommendations.
- Visualize and interpret the system’s performance to inform further improvements and real-world applicability.

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

## Modeling

## Evaluation

## Conclusion

## References
Kushmeeta, C., & Rout, S. K. (2013). Reading Habits - An Overview. IOSR Journal of Humanities and Social Science, 14(6), 14–15. http://www.iosrjournals.org/iosr-jhss/papers/Vol14-issue6/C01461317.pdf

OECD. (2023). PISA 2022 Results (Volume I and II): Country Notes: Indonesia. Organisation for Economic Co-operation and Development. https://www.oecd.org/en/publications/pisa-2022-results-volume-i-and-ii-country-notes_ed6fbcc5-en/indonesia_c2e1ae0e-en.html
