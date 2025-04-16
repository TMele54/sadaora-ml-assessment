# sadaora-ml-assessment
machine learning assessment

Looking for a dataset to use, candidates from kaggle and others are:

# Lifestyle and nursing datasets
1. https://www.kaggle.com/datasets/priyankraval/nurse-stress-prediction-wearable-sensors   stress
2. https://www.kaggle.com/datasets/antgoldbloom/doctors-and-nurses-per-1000-people-by-country
3. https://www.kaggle.com/datasets/cms/cms-pbj-daily-nurse-staggins-2017-2018
4. https://www.kaggle.com/datasets/thedevastator/permanent-nurse-outcomes-in-swiss-psychiatric-ho
5. https://www.kaggle.com/datasets/mexwell/nurse-care-activity-recognition
6. Not using but interesting ntl https://www.kaggle.com/datasets/willianoliveiragibin/causes-of-death-registered
7. https://www.kaggle.com/datasets/aravindnathan02/nurse-hourly-pay-rates-in-us-metros    compensation
8. https://www.kaggle.com/datasets/thedevastator/nursing-home-quality-staffing
9. https://www.kaggle.com/datasets/chiebukar/daily-nurse-staffing-2024-q2
10. https://www.kaggle.com/datasets/manuelcamachor/level-of-anxiety-in-nursing-during-covid-pandemic
11. https://www.kaggle.com/datasets/miadul/overstimulation-behavior-and-lifestyle-dataset
12. https://www.kaggle.com/datasets/anthonytherrien/half-a-million-lifestyle  
13. https://www.kaggle.com/datasets/abdelazizsami/lifestyle-dataset  20% of data are nurses
14. https://www.kaggle.com/datasets/hasaanrana/diet-exercise-and-pcos-insights maybe something your risk of
15. https://www.kaggle.com/datasets/iamsouravbanerjee/heart-attack-prediction-dataset your risk of
16. https://www.kaggle.com/datasets/rabieelkharoua/parkinsons-disease-dataset-analysis risk of
17. https://www.kaggle.com/datasets/shreyasparaj1/lung-cancer-dataset risk of
 

# Idea:
I took nurse working hours from https://data.cms.gov/quality-of-care/payroll-based-journal-daily-nurse-staffing/data
and explored their key trends. I discovered CNA, RN, and LPN were the highest percentage workers vs all nurses and even
other specialties like pharmacist or physicians assistant. Then for RN, LPN, and CNA, I compiled their worked hours as
a percentage of employee hours vs contract hours. If a nursing home had more contract hours than employee hours they
were labeled a contractor and vice versa. This created a supervised binary problem and so I chose a Logistic Regression
with LDA on a Standard scalar for normalization. I also tried a multi layered perceptron to see if there was any 
meaningful improvement in F1, accuracy, recall but the LR outperformed across the board on most runs. The data for this
project is too large for Github and easy sharing. I included code that can be used to download the files, but for ease
of sharing, I have saved the files in parquet file type.

# Why I chose this approach:



# Production
This model is lightweight and doesn't require extensive infrastructure to operate. Given the anticipated low traffic, 
I would deploy it on an EC2 instance—possibly a free-tier option like t2.micro—using python FastAPI, with Pydantic schemas 
to validate data and maintain data model integrity. To ensure stability, I would integrate  monitoring services that 
alert me to any server issues and configure Ubuntu to automatically restart the API if it stops running. 
Logging would be incorporated to capture user data and track performance metrics such as overall utilization. I would 
implement a blue-green deployment strategy to seamlessly swap new models into production without service interruptions, 
allowing for continuous improvement.

For a more robust and scalable approach, I would prioritize machine learning operations. Leveraging Apache Airflow,
I would streamline data movement through the API while maintaining version control and a feature store utilizing 
Pydantic models. To monitor the model’s classification accuracy—especially as nursing home trends evolve—I would
consider continuous training to keep it up to date. Deploying on edge servers would help mitigate DDoS attacks, while
Airflow would support ongoing monitoring and tracking of the model's performance over time.





