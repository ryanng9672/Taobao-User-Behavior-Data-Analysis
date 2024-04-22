# Taobao-User-Behavior-Data-Analysis
![___________________23-4-2024_22131_zizhunguo com](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/f3753e7a-4251-4f48-9d58-794ad22a0e3c)

# Table of Content
* [Project_Introduction](#Project_Introduction)
* [Analysis_Purpose](#Analysis_Purpose)
* [Python](#Python)
* [Power_bi](#Power_bi)
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

- data overview(In_python)
- Calculating Daily Page Views, Unique Visitors, and Pages per Visitor for Taobao Data
```shell
pv_data = filtered_taobao_df[filtered_taobao_df['behavior_type'] == 'pv']

pv_uv_puv = pv_data.groupby('date').agg(
    pv=('user_id', 'count'),  
    uv=('user_id', pd.Series.nunique), 
).reset_index()

pv_uv_puv['puv'] = round(pv_uv_puv['pv'] / pv_uv_puv['uv'], 1)
```
![螢幕擷取畫面 2024-04-23 030633](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/4f571eab-a1b5-4185-ae2b-8f0a94a35dfc)




# Power_bi




