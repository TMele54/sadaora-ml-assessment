# sadaora-ml-assessment
machine learning assessment

# Initial Research:

Lifestyle and nursing datasets
1. https://www.kaggle.com/datasets/priyankraval/nurse-stress-prediction-wearable-sensors   stress
2. https://www.kaggle.com/datasets/antgoldbloom/doctors-and-nurses-per-1000-people-by-country
3. https://www.kaggle.com/datasets/cms/cms-pbj-daily-nurse-staggins-2017-2018
4. https://www.kaggle.com/datasets/thedevastator/permanent-nurse-outcomes-in-swiss-psychiatric-ho
5. https://www.kaggle.com/datasets/mexwell/nurse-care-activity-recognition
6. https://www.kaggle.com/datasets/aravindnathan02/nurse-hourly-pay-rates-in-us-metros    compensation
7. https://www.kaggle.com/datasets/thedevastator/nursing-home-quality-staffing
8. https://www.kaggle.com/datasets/chiebukar/daily-nurse-staffing-2024-q2
9. https://www.kaggle.com/datasets/manuelcamachor/level-of-anxiety-in-nursing-during-covid-pandemic
10. https://www.kaggle.com/datasets/miadul/overstimulation-behavior-and-lifestyle-dataset
11. https://www.kaggle.com/datasets/anthonytherrien/half-a-million-lifestyle  
12. https://www.kaggle.com/datasets/abdelazizsami/lifestyle-dataset  20% of data are nurses
13. https://www.kaggle.com/datasets/hasaanrana/diet-exercise-and-pcos-insights maybe something your risk of
14. https://www.kaggle.com/datasets/iamsouravbanerjee/heart-attack-prediction-dataset your risk of
15. https://www.kaggle.com/datasets/rabieelkharoua/parkinsons-disease-dataset-analysis risk of
16. https://www.kaggle.com/datasets/shreyasparaj1/lung-cancer-dataset risk of

# I decided on

## Data Methodology
https://data.cms.gov/resources/payroll-based-journal-methodology-0

## Data Dictionaries / Glossaries / indexes
https://data.cms.gov/resources/payroll-based-journal-daily-non-nurse-staffing-data-dictionary
https://data.cms.gov/resources/payroll-based-journal-daily-nurse-staffing-data-dictionary

## Datasets
https://data.cms.gov/quality-of-care/payroll-based-journal-daily-non-nurse-staffing/data
https://data.cms.gov/quality-of-care/payroll-based-journal-daily-nurse-staffing/data
https://data.cms.gov/provider-data/dataset/4pq5-n9py#data-table

## Why approach and key insights:
I looked through the different nursing datasets on kaggle and ultimately found cms. I felt the data was more representative
of what a real world use case might reflect, for example the 44k rows of ~40 columns of nurse working hours all gets turned 
into just one column of a user defined metric as opposed to a synthetic kaggle dataset that has a nice target column.

I took nurse working hours from https://data.cms.gov/quality-of-care/payroll-based-journal-daily-nurse-staffing/data
and explored the key trends. I discovered CNA, RN, and LPN were the highest percentage workers vs all nurses and even
other specialties like pharmacist or physicians assistant. Then for RN, LPN, and CNA, I compiled their worked hours as
a percentage of employee hours vs contract hours. If a nursing home had more contract hours than employee hours they
were labeled a contractor and same for employees. I then enriched this classification column with another dataset just about the 
facility. There are many things that could be done with this enrichment set, but I simply want to use it as a way of
showing the process, but there is a lot to it and a lot could be done with it. For each facility for each of the 3 months
of data I created the classification column, and then merged them vertically. This created a supervised binary problem 
- I chose a Logistic Regression with LDA on a Robust scalar for normalization. 

The Robust scaler, to help smooth out outliers in the data. The outliers were more likely eliminated in the final ML Dataset
when I balanced it, if they did slip in, then they could significantly affect a standard or minmax scaler. I used LDA Linear Discriminant
Analysis as a preprocessing step to lower the dimensionality of my data. Being a binary problem limits this, but it still improved performance.

I used a binary logistic regression:
    I do not have a linear relationship between independent and dependent variables
    Dependent variable is inherently binary (contractor or employee)
    Observations are independent of each other (nursing homes)
    These nursing homes are not highly correlated with each other.

I also tried a multi layered perceptron to see if there was any meaningful improvement in F1, accuracy, recall but the LR outperformed across the
project is too large for Github and easy sharing. I included code that can be used to download the files, but for ease
of sharing, I have saved the files in parquet file type.

## Logistic regression
- Precision: 0.7291666666666666
- Recall: 0.75
- F1 Score: 0.7394366197183099
- Confusion Matrix:
- [[101  39]
-  [ 35 105]]
- ROC AUC: 0.7357142857142857
- Accuracy with lda: 0.7357142857142858


## MLP:
- Precision: 0.7096774193548387
- Recall: 0.8148148148148148
- F1 Score: 0.7586206896551724
- Confusion Matrix:
- [[100  45]
-  [ 25 110]]
- ROC AUC: 0.752234993614304
- Accuracy with lda: 0.75


## Production
This model is lightweight and doesn't require extensive infrastructure to operate. Given the anticipated low traffic, 
I would deploy it on an EC2 instance a free tier one like t2.micro w/ python FastAPI, Pydantic schemas 
to validate data and maintain data model integrity. To ensure stability, I would integrate monitoring services that 
alert me to any server issues and configure Ubuntu to automatically restart the API if it stops running. 
Logging would be incorporated to capture user data and track performance metrics such as overall utilization. I would 
implement a blue-green deployment strategy to seamlessly swap new models into production without service interruptions, 
allowing for continuous improvement.

For a more robust and scalable approach, I would prioritize machine learning operations. Leveraging Apache Airflow,
I would streamline data movement through the API while maintaining version control and a feature store utilizing 
Pydantic models. To monitor the model’s classification accuracy, especially as nursing home usage changes, I would
consider continuous training to keep it current. Deploying on edge servers would help mitigate DDoS attacks, while
Airflow would support ongoing monitoring and tracking of the model's performance over time.





