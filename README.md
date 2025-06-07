# Machine Learning Project Report - Kevin Juan Carlos

## Project Overview
Reading books offers numerous benefits for both individual and societal development, such as enhancing literacy skills, broadening knowledge, strengthing critical thinking, and fostering character and social empathy. According to Kushmeeta dan Rout (2013), cultivating strong reading habits from an early age is essential, as it significantly impacts children's mental growth and academic success. Children who read for pleasure unconsciously improve their reading proficiency and language skills (Reyhene, 1998; Bignold, 2003). However, Reading literacy level in Indonesia remains a cause for concern.

Based on the 2022 PISA results released by the OECD, only about 25% of Indonesian students achieved Level 2 or higher in reading—significantly below the OECD average of 74%. Level 2 denotes fundamental skills such as identifying the main idea in a text and locating information based on explicit criteria. Even more concerning is the near absence of Indonesian students reaching Level 5 or above, which reflects the ability to comprehend lengthy and complex texts and to distinguish fact from opinion using implicit cues. This low literacy rate has a profound impact on the quality of human resources and the nation’s competitiveness. Literacy serves as the cornerstone for educational advancement, productivity, and active participation in society. Without a strong reading culture, individuals will struggle to adapt to the demands of the digital age and globalization.

One viable solution to address this issue is the development of a book recommendation system. Such a system can assist readers in discovering books tailored to their interests, age, and preferences, thereby fostering greater engagement with reading activities. Through a personalized and adaptive approach, this system holds the potential to boost reading motivation and cultivate sustainable reading habits.

## Business Understanding
### Problem Statements
### Goals
### Solution Statements

## Data Understanding
### Source
The dataset used in this project was obtained from [Kaggle](https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews) with the name "Diabetes Prediction Dataset". It can be downloaded using kagglehub library with the following code:
```
import kagglehub

path = kagglehub.dataset_download("mohamedbakhet/amazon-books-reviews")
```
### Data Distribution Visualization
This part presenting visualization of the feature.

- #### Boxplot for review/score Distribution
![review/score](https://github.com/user-attachments/assets/bd77b2e3-183a-4836-9b6b-c6bfa5c47ac8)

Based on the boxplot, distribution of review/score is positively skewed (right-skewed). However, since the data will be used to generate recommendations that prioritize higher ratings, removing outlier may not be necessary.

## Data Preparation

## Modeling

## Evaluation

## Conclusion

## References
Kushmeeta, C., & Rout, S. K. (2013). Reading Habits - An Overview. IOSR Journal of Humanities and Social Science, 14(6), 14–15. http://www.iosrjournals.org/iosr-jhss/papers/Vol14-issue6/C01461317.pdf

OECD. (2023). PISA 2022 Results (Volume I and II): Country Notes: Indonesia. Organisation for Economic Co-operation and Development. https://www.oecd.org/en/publications/pisa-2022-results-volume-i-and-ii-country-notes_ed6fbcc5-en/indonesia_c2e1ae0e-en.html
