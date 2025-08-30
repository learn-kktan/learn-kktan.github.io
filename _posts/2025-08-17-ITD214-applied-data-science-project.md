---
layout: post
author: Tan Kek Koon (1653601c)
title: "NYP ITD214 Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background
<img width="150" height="150" alt="image" src="https://github.com/user-attachments/assets/d1989408-98ce-49fb-8b8d-0b96a875772c" /><br />
Mental health issues such as depression, anxiety, and burnout remain prevalent, particularly among younger generation. Despite growing awareness,  stigma around mental illness still persists and generally people still see mental illness as sign of weakness and shun discussions on mental illness.
Mental health issues often go unnoticed early, only to be identified later when treatment becomes more resource-intensive.

### Main Business Goals
To develop a data-driven solution that identifies early warning signs and key contributing factors of mental health challenges among adults, with the aim of enabling timely intervention and reducing the risk of long-term mental health deterioration.

After crawling the web for several related dataset, Zenodo Mental Health and Lifestyle Dataset for Sentiment Analysis is selected for analysing the workplace impact.

The dataset (version v2, Oct 5, 2024) can be downloaded from https://zenodo.org/records/14838680

It is a 5.3MB size with 50000 records with 17 columns (User_ID, Age, Gender, Occupation, Country, Mental_Health_Condition, Severity, Consultation_History, **Stress_Level**, Sleep_Hours, Work_Hours, Physical_Activity_Hours, Social_Media_Usage, Diet_Quality, Smoking_Habit, Alcohol_Consumption, Medication_Usage).

### Sub-objective Number 3: Workplace Impact
<img width="179" height="179" alt="image" src="https://github.com/user-attachments/assets/0624a702-17e7-40a9-94f8-1d886309d7d3" />

To predict the likelihood of an individual experiencing **high stress levels** based on their occupation, work hours and other various factors.

The target variable is the **stress level** with ordinal multi-class values of low, medium and high.

## Work Accomplished

### Data Preparation
1. Firstly, exploratory data analysis (EDA) is conducted to understand the respective columns.
2. The stress level frequency is evenly distributed in the low, medium and high categories. It is found that the stress level is also unusually evenly distributed across the occupation, countries, working hours, sleep hours and physical activity.
3. Working hours per week (max: 80, min: 30), sleep hours per day (max: 10, min: 4), social media usage per day (max: 6, min: 0.5), physical activity per week (max: 10, min: 0).
4. From statistics shown above, there were signs of overworking, lack of proper sleep, overusage of social media and no physical activity/exercising.
5. No duplicates were found.
6. The total number of working hours plus sleeping hours, physical activity and social media usage exceeded the maximum number of hours per week (24 * 7 = 168 hours per week). Thus, only records of the total hours (working hours + sleep hours + physical activity + social media usage) that less than 90% of 168 hours are retained.
7. The ages are bin to groups of 18-25, 26-35, 36-45, 46-55, 56-65, 66+.
8. The sleeping hours and social media usage are standarised to weekly hours to be consistent with the working hours and physical activity.
9. The values in the severity (of the mental health condition) are updated to none if the mental health condition is no and the original value is null value.
10. The values in gender are replaced to "Unknown" if it is "non-binary" or "prefer not to say".
11. Columns "Person_ID" and "Age" are removed as they are non-relevant for modelling.

### Modelling
As the target variable, stress level is an ordinal multi-class, the 4 following modelling techniques are selected to predict which factors are most likely to affect the stress level.
- Logistics regression (whitebox)
- Decision tree (whitebox)
- Random forest (blackbox)
- Deep learning Tabnet Classifier (blackbox)

**Data Splitting Processes**
- After the data is cleansed, only 40927 out of 50000 rows are left in the dataset.
- One-hot encoding is performed on all the categorical columns except for the target variable.
- Random state for all the modelling is set to 123.
- The dataset is split into 60% for training, 20% for validation and 20% for testing. Stratify is used in the splitting to ensure that the data split will preserve the same proportion of classes in the target variable accross both the training and testing sets. This provide more a robust and realistic evaluation of the models' performance.
- After splitting, fit_transform() is applied on the training data to learn the transformation parameters and apply the transformation. transform() is applied on the testing data to apply the same transformation learning from the training data without recalculating the parameters.

#### Model 1: Logistic regression
In the logistic regression model, ibfgs solver (optimization algorithms) and maximum iteration as 1000 are setup.

**Confusion Matrix:** <br />
The actual labels and predicted labels are ~33% respectively.<br />
<img width="516" height="432" alt="image" src="https://github.com/user-attachments/assets/945b1aaf-13ed-4bcb-8138-cf6a1457ffc9" />

**AUC:** <br />
High (50%), low (50%), medium (50%) are having a random guess linear line. <br />
<img width="846" height="701" alt="image" src="https://github.com/user-attachments/assets/1ac6eb68-ad42-4fd8-83e4-d03c926c1f7c" />


**Top feature Importance for high stress (with logistic regression):**
1. High number of sleep hours.
2. Uses social social media.
3. Has mental health condition.
4. Lives in USA or Germany.
<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/00d94741-6112-4a96-9a59-05839e92bf9d" />

**Top 5 feature Importance for low stress (with logistic regression):**
1. Likely to be the age of 26 to 35
2. Likely to be the age of 46 to 55
3. Occupation is IT/finance.
4. Lives in Canada.
5. Physical activity.
<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/1bd366ad-d82b-4037-b5f3-7d62b0553f03" />


#### Model 2: Decision tree
In the decision tree model, a maximum of 5 in the depth is setup.

**Confusion Matrix:** <br/>
The actual labels and predicted labels are ~33% respectively.<br />
<img width="515" height="432" alt="image" src="https://github.com/user-attachments/assets/6c3da1de-54e9-496a-9f64-0e05590057b6" />

**AUC:** <br />
High (50%), low (50%), medium (50%) are having a random guess linear line. <br />
<img width="846" height="701" alt="image" src="https://github.com/user-attachments/assets/1e4ddd30-0397-4c1d-b9ee-9058fd163ae5" />

**Top 5 features importance (Decision Tree):**
1. Number of sleep hours
2. Number of social media usage
3. Number of work hours
4. Number of physical activity
5. Severity of the mental health condition is low
<img width="1069" height="701" alt="image" src="https://github.com/user-attachments/assets/90bcfba2-9ee2-4e7b-af9c-26b8dbb30db2" />

**Trees in Decision Tree**
- On the top of the tree, it is splitted by the consultation history.
- On the left side of split (with consulted history), factors such as other country, Canada, number of sleep hours contribute to high stress.
- On the right side of split (with no consulted history), factors such as number of physical activity (low), number of social media usage (high) contribute to high stress.
<img width="1574" height="812" alt="image" src="https://github.com/user-attachments/assets/56ec3a4a-d172-4f70-b5ab-8046fd391a9e" />


#### Model 3: Random forest
In the random forest model, 500 estimators and a maximum depth of 5 are setup.

**Confusion matrix:** <br />
The actual labels and predicted labels are ~33% respectively.<br />
<img width="515" height="432" alt="image" src="https://github.com/user-attachments/assets/48141672-c3f0-4e49-8f6d-e3a9077b91ca" />

**AUC:** <br />
High (50%), low (50%), medium (50%) are having a random guess linear line.
<img width="846" height="701" alt="image" src="https://github.com/user-attachments/assets/dc087e14-1d08-4c83-baeb-e2707a102da3" />

**Top feature importance (Random Forest):**
1. Number of social media usage
2. Number of sleep hours
3. Number of work hours
4. Number of physical activity
5. Consultation history
<img width="1069" height="701" alt="image" src="https://github.com/user-attachments/assets/c6339f16-810f-4f47-96c0-1ace459710ab" />

**SHAP (SHapley Additive exPlanations) Plot for High Level of Stress**

Blue dots: Low feature value<br />
Red dot: High feature value<br />

Insights from SHAP Plot:
- High number of social media usage.
- Long working hours.
- Works as IT.
- Lives in USA.
- Sleeps for long hours.
- Spends little time in physical activity.

<img width="799" height="940" alt="image" src="https://github.com/user-attachments/assets/550e9f6d-df2e-49ca-a360-613e7e0ad923" />


#### Model 4: Deep learning network (Tabnet Classifer)
Tabnet is a deep learning architecture that is designed to be used for tabular data.
In the Tabnet classifier, adam optimizer, learning rate of 0.02, step learning rate scheduler, for every 10 epochs, the learning rate is multipled by 0.9 are setup.

**Top 5 feature importance (Tabnet):**
1. Social alcohol drinker.
2. Lives in USA/Germany.
3. Diet is unhealthy.
4. Occupation is IT.
5. Is taking medication for mental health.
<img width="991" height="590" alt="image" src="https://github.com/user-attachments/assets/9f44b85c-7204-42f5-b447-ce55c78b07dc" />


### Evaluation

Model/Metrics         | Logistic Regression   |     Decision Tree     |     Random Forest    |   TabNet Classifer   |
--------------------- | --------------------- | --------------------- | ---------------------|----------------------|
Accuracy              |         33%           |         33%           |          33%         |        33%           |
Precision             | 34% (High) 33% (Low)  | 33% (High) 32% (Low)  | 33% (High) 32% (Low) | 34% (High) 23% (Low) |
Recall                | 33% (High) 22% (Low)  | 54% (High) 11% (Low)  | 54% (High) 11% (Low) | 42% (High) 0% (Low) |

As our main goal is to detect the high level of stress, we will use the highest score value in the recall to prevent false negatives where the patient is having a high level of stress but predicted as not stressed.
Decision tree and random forest will be selected as the models to predict the high level of stress in the workplace. Decision tree can be used to drill-in to gain more insights of the actual values in the factors.

From the 4 models discussed above, it can be shown that the most important and common features are
- Social media usage
- Sleep hours
- Work hours
- Physical activity

## Recommendation and Analysis

**Business recommendation:** <br />
- For detection of high level of stress in the workplace, we can identify it from the number of sleep hours, long working hours and high usage of social media.
- Company should cap the maximum number of working hours per week for work-life balance. Social media platforms (FB, Instagram, TikTok) can program algorithms to detect stress patterns from the heavy usage/posts made and send out mental health advisory to users.

**Data analysis** <br />
- From the low accuracy (33%) and linear line (50%) in the area under curve (AOC) shown in the 4 models, the features in the dataset might not be able to relate to the target variable, level of stress or contain many noise.
- The poor quality data could be manipulated because the distribution is too perfectly evenly distributed across all columns.

## AI Ethics
Discussion of the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in the project. 

- Tbere is no personal data in the dataset thus there is no privacy issue. Masking/removal of data is not required during the data preparation stage.
- The gender column could be removed from the modelling to ensure that there is fairness and no bias.
- The data has signs of error in the high working hours (80 hours per week) that lead to the low accuracy in modelling.
- Accountability..????
- Proper documentation of the data source, processing steps and modelling are done to ensure there is transparency.

## Source Codes and Datasets
This is the <a href="https://github.com/learn-kktan/ITD214_Project" target="_blank">link</a> to the source code of the modelling and dataset.
https://github.com/learn-kktan/ITD214_Project
