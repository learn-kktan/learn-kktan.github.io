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

The dataset (version v2, Oct 5, 2024) is downloaded from https://zenodo.org/records/14838680

It is 5.3MB size with 50000 records with 17 columns (User_ID, Age, Gender, Occupation, Country, Mental_Health_Condition, Severity, Consultation_History, Stress_Level, Sleep_Hours, Work_Hours, Physical_Activity_Hours, Social_Media_Usage, Diet_Quality, Smoking_Habit, Alcohol_Consumption, Medication_Usage).

### Sub-objective Number 3: Workplace Impact
<img width="179" height="179" alt="image" src="https://github.com/user-attachments/assets/0624a702-17e7-40a9-94f8-1d886309d7d3" />

To predict the likelihood of an individual experiencing high stress levels based on their occupation, work hours and other various factors.

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
8. The sleeping hours and social media usage are standarised to weekly hours.
9. The values in the severity (of the mental health condition) are updated to none if the mental health condition is no and the original value is null value.
10. The values in gender are replaced to "Unknown" if it is "non-binary" or "prefer not to say".
11. Columns "Person_ID" and "Age" are removed as they are non-relevant for modelling.

### Modelling
As the target variable, stress level is an ordinal multi-class, the 4 following modelling techniques are selected to predict which factors are most likely to affect the stress level.
- Logistics regression (whitebox)
- Decision tree (whitebox)
- Random forest (blackbox)
- Deep learning network (Tabnet)

**Data Splitting Processes**
- After the data cleansing processes, only 40927 rows are left in the dataset.
- The dataset is split into 60% for training, 20% for validation and 20% for testing. Stratify is used in the splitting to ensure that the data split should preserve the same proportion of classes in the target variable accross both the training and testing sets. This provide more a robust and realistic evaluation of the models' performance.
- After splitting, fit_transform() is applied on the training data to learn the transformation parameters and apply the transformation. transform() is applied on the testing data to apply the same transformation learning from the training data without recalculating the parameters.

#### Logistic regression

- In the logistic regression model, ibfgs solver (optimization algorithms) and maximum iteration as 1000 are setup.

**Confusion Matrix:**
The actual labels and predicted labels are ~33% respectively.
<img width="516" height="432" alt="image" src="https://github.com/user-attachments/assets/945b1aaf-13ed-4bcb-8138-cf6a1457ffc9" />

**AUC:**
<img width="846" height="701" alt="image" src="https://github.com/user-attachments/assets/1ac6eb68-ad42-4fd8-83e4-d03c926c1f7c" />

**Top feature Importance for high stress:**
1. High number of sleep hours.
2. Uses social social media.
3. Has mental health condition.
4. Lives in USA or Germany.

<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/00d94741-6112-4a96-9a59-05839e92bf9d" />

**Top 5 feature Importance for low stress:**
1. Likely to be the age of 26 to 35
2. Likely to be the age of 46 to 55
3. Occupation is IT of finance.
4. Lives in Canada.
5. Physical activity.
<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/1bd366ad-d82b-4037-b5f3-7d62b0553f03" />


#### Decision tree

#### Random forest


#### Deep learning network (Tabnet)

### Evaluation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Recommendation and Analysis
Explain the analysis and recommendations

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Source Codes and Datasets
This is the <a href="https://github.com/learn-kktan/ITD214_Project" target="_blank">link</a> to the source code of the modelling and dataset.
https://github.com/learn-kktan/ITD214_Project
