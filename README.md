# Taobao-User-Behavior-Data-Analysis
![___________________23-4-2024_22131_zizhunguo com](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/f3753e7a-4251-4f48-9d58-794ad22a0e3c)

# Table of Content
* [Project_Introduction](#Project_Introduction)
* [Analysis_Purpose](#Analysis_Purpose)
* [Python](#Python)
* [RFＭ](#RFＭ)
* [Power_bi](#Power_bi)
* For those who prefer to see the analysis outcomes without delving into the coding details, please click on [Data_Analysis_Results](#Data_Analysis_Results).
* [WebsiteRelated](#WebsiteRelated)

# Project_Introduction
* **Overview** 

UserBehavior is a dataset offered by Alibaba, containing user behavior records from Taobao. It serves as a valuable resource for researching implicit feedback recommendation problems. The dataset spans from November 25, 2017, to December 3, 2017, and includes 1,001,506 behavior samples from approximately 987,994 distinct users, 4,162,024 unique products, and 3,623 different product categories. The behaviors captured include clicks, purchases, additions to cart.

For my analysis using Python and Power BI, this dataset will provide valuable insights into user interactions within the Taobao platform.  🚀🔍


* **Introduction** [data set link](https://tianchi.aliyun.com/dataset/649)

![螢幕擷取畫面 2024-04-23 021757](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/e9f8bc0d-2ff5-4712-9d05-c23544bce467)


The dataset includes all activities of approximately one million random users who were active between November 25, 2017, and December 3, 2017. The activities include clicking, purchasing, adding to cart, and liking. The organization of the dataset is similar to MovieLens-20M, where each row represents a user action consisting of User ID, Item ID, Category ID, Behavior Type, and Timestamp, separated by commas. Detailed descriptions of each column in the dataset are as follows:

![螢幕擷取畫面 2024-04-23 022607](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/38326290-cdef-424a-bb45-a66011bf9e11)



Note that there are four types of user behaviors, which are:

![螢幕擷取畫面 2024-04-23 022615](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/d77b6704-c1d2-4539-a334-c5c47a69776c)


# Analysis_Purpose

- Optimize implicit feedback recommendation systems

![tabo](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/51fed9f4-3575-4437-8582-25b60329dacf)

* **AARRR model**

![aarrr-growth-funnel-model-infographic-template-with-icons-has-5-steps-such-as-acquisition-activation-retention-referral-and-revenue-pirate-metrix-or-pirate-framework-to-measure-growth-and-success-vector](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/5783d81d-54c2-4c41-a144-44c8f6aed8d1)

## 2. Data Source
- Original data: 100 million user behavior records
- Sampled data: 1 million records

## 3. Analysis Tools
- Python
- Power_bi

## 4. Implementation Steps
- Data Preparation and Cleaning
- Data Sampling
- Execution of Data Analysis
- Interpretation of Results
- Recommendations for Recommendation System Optimization

## 5. Expected Outcomes
- Improved Efficiency of Recommendation System
- Enhanced User Experience
- Increase in Sales

# Python
- data_cleaning_part
```shell
print("check_taobao_df how may row",taobao_df.shape)
taobao_df.drop_duplicates()
print("drop_duplicates to taobao_df",taobao_df.shape)
```
![螢幕擷取畫面 2024-04-23 024822](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/24670744-d313-4c46-8d32-beb4c51606c1)

```shell
taobao_df.isnull().sum()
```
![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/2a7fbef1-e7d7-44dd-a6a1-baed658c28a9)

- set_up_time(hour,week,time)
```shell
def to_date(dt):
    return dt.date()

def to_time(dt):
    return dt.time()

def getWeek(dt):
    return weekDict[dt.weekday()]

weekDict = {0: 'Mon', 1: 'Tue', 2: 'Wed', 3: 'Thur', 4: 'Fri', 5: 'Sat', 6: 'Sun'}
taobao_df['date'] = taobao_df['timestamp'].apply(to_date)
taobao_df['time'] = taobao_df['timestamp'].apply(to_time)
taobao_df['week'] = taobao_df['timestamp'].apply(getWeek)
```
![螢幕擷取畫面 2024-04-23 030131](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/4463d8e8-6e73-46e9-ab23-f00181ad5250)

- check the time range
```shell
taobao_df['date'] = pd.to_datetime(taobao_df['date'])
start_date = pd.to_datetime('2017-11-25')
end_date = pd.to_datetime('2017-12-03')

filtered_taobao_df = taobao_df[(taobao_df['date'] >= start_date) & (taobao_df['date'] <= end_date)]
```
![螢幕擷取畫面 2024-04-23 025935](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/9237f3a5-9137-4f41-bdff-15d19cc689e5)

- **data overview(In_python)**
```shell
#Calculating Daily Page Views, Unique Visitors, and Pages per Visitor for Taobao Data
pv_data = filtered_taobao_df[filtered_taobao_df['behavior_type'] == 'pv']

pv_uv_puv = pv_data.groupby('date').agg(
    pv=('user_id', 'count'),  
    uv=('user_id', pd.Series.nunique), 
).reset_index()

pv_uv_puv['puv'] = round(pv_uv_puv['pv'] / pv_uv_puv['uv'], 1)
```
![螢幕擷取畫面 2024-04-23 030633](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/4f571eab-a1b5-4185-ae2b-8f0a94a35dfc)

```shell
#Daily PV and UV 
import matplotlib.pyplot as plt

pv_uv_puv['date'] = pd.to_datetime(pv_uv_puv['date'])

plt.figure(figsize=(10, 6))

plt.plot(pv_uv_puv['date'], pv_uv_puv['pv'], label='PV', marker='o')

plt.plot(pv_uv_puv['date'], pv_uv_puv['uv'], label='UV', marker='o')

plt.title('Daily PV and UV')
plt.xlabel('Date')
plt.ylabel('Counts')
plt.legend()
plt.grid(True)
plt.xticks(rotation=45) 
plt.tight_layout()
plt.show()
```
![output](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/f28e1e03-2fbd-4bc5-97bb-2a1eed65a7d5)

```shell
#Taobao User Retention Calculation
filtered_taobao_df['dates'] = pd.to_datetime(filtered_taobao_df['date'])

user_dates = filtered_taobao_df.groupby(['user_id', 'dates']).size().reset_index(name='counts')

user_dates = user_dates.merge(user_dates, on='user_id', suffixes=('_a', '_b'))

user_dates['date_diff'] = (user_dates['dates_b'] - user_dates['dates_a']).dt.days

next_day = user_dates[user_dates['date_diff'] == 1].groupby('dates_a').size()
same_day = user_dates[user_dates['date_diff'] == 0].groupby('dates_a').size()

retention_rate = (next_day / same_day).reset_index()
retention_rate.columns = ['dates', 'retention_1']

retention_rate['dates'] = retention_rate['dates'].dt.strftime('%Y-%m-%d')

print(retention_rate)
```
![螢幕擷取畫面 2024-04-23 090324](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/09221d25-b849-4409-9bea-84f2480e543d)

```shell
#Bounce users & Bounce Rate
user_behavior_count = filtered_taobao_df.groupby('user_id').size()

# find one time users
bounce_users = user_behavior_count[user_behavior_count == 1].count()

# check the total_pv
total_pv = pv_uv_puv['pv'].sum()

# check the % of  bounce users
bounce_rate = bounce_users / total_pv

# 输出结果
print("流失用户:", bounce_users)
print("Total_pv:", total_pv)
print("流失率:", bounce_rate)
```
![螢幕擷取畫面 2024-04-23 090558](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/2006a702-87b0-457a-83f8-d9887341e406)
# RFＭ
```shell
#In RFM model we use the 10000000 to testing
df_sample = taobao_df.sample(n=10000000,random_state=42)
```
![螢幕擷取畫面 2024-04-23 091708](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/b8b53bf6-c3a6-48b1-8e8e-d894ec248794)

- model set up!!!

![螢幕擷取畫面 2024-04-23 091831](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/90b465c1-d6fc-41ec-acde-ca0383e0b695)

```shell
result in rfm model
```
![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/f65d2fec-dc37-4355-b782-ee8a71c6edd7)

```shell
#RFM Analysis for Taobao Purchase Data(set up point)
#because the data set don't have the Monetary ($$$) so i just use the R & F 
df_purchases = filtered_taobao_df[filtered_taobao_df['behavior_type'] == 'buy']

rfm = df_purchases.groupby('user_id').agg({
    'timestamp': lambda x: (datetime.now() - x.max()).days,  
    'user_id': 'count'   
}).rename(columns={'timestamp': 'recency', 'user_id': 'frequency'})

rfm['recency_score'] = pd.qcut(rfm['recency'], 5, labels=[5, 4, 3, 2, 1])  
rfm['frequency_score'] = pd.qcut(rfm['frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5])
```
![螢幕擷取畫面 2024-04-23 092237](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/8b0c3873-1c2f-408e-b9aa-f433310bbae2)

```shell
#Classification of Taobao Users Based on RFM Scores
def calculate_fscore(x):
    if x >= 100:
        return 5
    elif 50 <= x < 100:
        return 4
    elif 20 <= x < 50:
        return 3
    elif 5 <= x < 20:
        return 2
    else:
        return 1

rfm['fscore'] = rfm['frequency'].apply(calculate_fscore)

rfm['rscore'] = pd.qcut(rfm['recency'], 5, labels=[5, 4, 3, 2, 1]).astype(int)

def classify_user(row):
    if row['fscore'] <= 2 and 1 <= row['rscore'] <= 2:
        return 'Bronze'
    elif row['fscore'] <= 3 and 2 <= row['rscore'] <= 3:
        return 'Silver'
    elif row['fscore'] <= 4 and 3 <= row['rscore'] <= 4:
        return 'Gold'
    else:
        return 'Diamond Member'

rfm['class'] = rfm.apply(classify_user, axis=1)
```
![螢幕擷取畫面 2024-04-23 092853](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/3e774954-6e82-4481-9d69-3ba0fcab21ea)
![rfm pie](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/6b72e38a-78f9-412d-8ccd-baa65c7f27ab)


# Power_bi
- **Guest Behavior**
- **clicks and active users are mainly concentrated in two time periods, 11:00-17:00 and 20:00-00:00. while the highest BUY crowd is 9:00-1700 the total number of visits in these two time periods is significantly higher than any other time periods. These two time slots are centered on 14:00 and 21:00, with 21:00 being the active peak of the day.**
- **Weekend sales compared to normal weekday**
![螢幕擷取畫面 2024-04-23 093217](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/1eb64e94-79d0-42ef-83fd-678a581674f2)

- **Conversion Rate &Bounce Rate**
![螢幕擷取畫面 2024-04-23 093317](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/d3b8a496-c199-46a3-b039-eba930660b73)

- **Are the product categories ranked consistently for views and purchases?**
![螢幕擷取畫面 2024-04-23 093331](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/677de169-b761-4af3-b54e-3f76ed4466d7)

- **Repeat rate and single purchase**
![螢幕擷取畫面 2024-04-23 093349](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/82e1bcb6-5ac9-49da-a0fa-7e6858d7ba55)



# Data_analysis_Results


# WebsiteRelated
- data set link 
https://tianchi.aliyun.com/dataset/649





