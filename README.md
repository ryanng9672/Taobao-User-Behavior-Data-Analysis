# Taobao-User-Behavior-Data-Analysis
![___________________23-4-2024_22131_zizhunguo com](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/f3753e7a-4251-4f48-9d58-794ad22a0e3c)

# Table of Content
* [Project_Introduction](#Project_Introduction)
* [Analysis_Purpose](#Analysis_Purpose)
* [Python](#Python)
* [RFＭ](#RFＭ)
* [Power_bi](#Power_bi).There will be diagrams to explain
* [Data_Analysis_Results](#Data_Analysis_Results).For those who prefer to see the analysis outcomes without delving into the coding details, please click here
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
![螢幕擷取畫面 2024-04-23 093217](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/1eb64e94-79d0-42ef-83fd-678a581674f2)
- **clicks and active users are mainly concentrated in two time periods, 11:00-17:00 and 20:00-00:00. while the highest BUY crowd is 9:00-1700 the total number of visits in these two time periods is significantly higher than any other time periods. These two time slots are centered on 14:00 and 21:00, with 21:00 being the active peak of the day.**
- **Weekend sales compared to normal weekday**
- **User behavior includes clicking, put into the shopping cart, collection and purchase, clicking accounted for 90% of the total number of behavior, and put into the shopping cart accounted for only 8%, and finally the actual purchase accounted for only 2%, the pinch point in the put into the cart and fav of this link, there may be a reason is that the user spends a lot of time looking for the right product for this can be optimized for the e-commerce platform's filtering function, so that the user can be more easily to find the right product.**
  
- **Explanation of Conversion Rates**
- **PV TO CAR&FAV Conversion Rate: 9.39%**
**This conversion rate indicates the percentage of all page views (PV) that result in users adding products to their shopping cart or favorites list (CAR&FAV), which is 9.39%.**
**A lower conversion rate may suggest that although many users visit the website, only a small fraction are interested enough in the products to consider a future purchase or to save the product details.**

- **CAR&FAV TO BUY Conversion Rate: 23.94%**
**This conversion rate represents the percentage of instances where products added to the shopping cart or favorites list (CAR&FAV) lead to an actual purchase (BUY), which is 23.94%.**
**A higher conversion rate indicates that among users who have shown interest in products and taken some action, a relatively high proportion decide to make the final purchase.**

- **Significance and Potential Impact**
**User Engagement and Purchase Decisions: A low PV to CAR&FAV conversion rate might mean that the presentation of pages or products is not effective enough in engaging users to take further action. This could be due to poor user interface design, insufficient product information, non-competitive pricing, or a lack of sufficient incentives.**

- **Behavior of High-Intent Buyers: A relatively high CAR&FAV to BUY conversion rate suggests that once users consider purchasing, they often complete the purchase process. This could be due to effective marketing strategies, attractive pricing policies, trust-building measures (such as customer reviews and clear return policies), and an optimized checkout process.**

- **Recommendations**
**Optimize Website and Product Pages: Improve the user interface and provide more detailed product information to increase the conversion from browsing to adding to the shopping cart or favorites.
Enhance User Engagement: Stimulate users' interest in purchasing through marketing campaigns, coupons, time-limited discounts, and other strategies.
Simplify the Purchase Process: Ensure the purchasing process is simple and quick, reducing barriers that might lead users to abandon the shopping cart.
By implementing these strategies, it is possible to improve the PV to CAR&FAV conversion rate, while maintaining or enhancing the already relatively high CAR&FAV to BUY conversion rate.**


- **Conversion Rate &Bounce Rate**
![螢幕擷取畫面 2024-04-23 093317](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/d3b8a496-c199-46a3-b039-eba930660b73)
- **Total Page Views (Totalpv): 89,660,688
 This metric indicates the total number of times that pages on the website have been viewed during the reported period. A high number of page views can suggest high traffic but does not necessarily indicate effective engagement or successful conversions.**

- **Bounce Rate: 98.98%
The bounce rate is exceptionally high, indicating that 98.98% of visits consist of a single page view, after which the visitor leaves the site without browsing further. This could suggest that the website is not engaging enough, or the landing pages are not effectively capturing the interest of the visitors. High bounce rates can also be linked to poor user experience, irrelevant content, or slow page load times.**

- **Cart Include Buy Conversion Rate: 0.92%
This conversion rate measures how many visitors who added items to their shopping cart proceeded to complete a purchase. A rate of 0.92% is quite low, suggesting that while some users show initial interest by adding products to their cart, very few follow through to make a purchase. This could be due to various factors such as high pricing, lack of trust, complex checkout processes, or inadequate product information.**

- **Fav Include Buy Conversion Rate: 1.11%
Slightly better than the cart conversion rate, this metric shows that 1.11% of users who added items to their favorites list eventually made a purchase. Although higher than the cart conversion rate, it still indicates a low overall effectiveness in converting interested users into buyers.**
- **Buy Conversion Rate: 2.16%
This rate indicates that out of all visitors to the site, 2.16% completed a purchase. Considering the high bounce rate and low engagement metrics in other areas, this conversion rate is relatively low but not uncommon for e-commerce sites, depending on the industry standard and niche market.**

- **Direct Purchase Path Predominates:
The "PV > CART > BUY" pathway represents a user journey from Page View (PV), to adding items to the Cart (CART), and then going straight to Purchase (BUY). This being the most common path suggests that many users quickly decide to buy after viewing the product. This indicates that the website effectively showcases its products and that the checkout process is likely streamlined, minimizing distractions or reasons for users to abandon their carts.**

- **Favorites as an Intermediate Step:
The second most common path, "PV > CART > FAV > BUY," involves users adding products to their cart, then adding them to their favorites (FAV), and eventually making a purchase. This indicates that a segment of the customer base prefers to take more time making their buying decisions, possibly using the favorites function to compare different options or to reconsider before finalizing the purchase.**

- **Decision Time and User Confidence:
Comparing the two paths, it can be inferred that users who go directly to purchase are likely more confident about their buying decision or feel a sense of urgency. In contrast, those who use the favorites feature may require more information or exhibit more hesitation before committing to the purchase. This could also reflect a preference among certain users to save items for later consideration.**


- **Are the product categories ranked consistently for views and purchases?**
![螢幕擷取畫面 2024-04-23 093331](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/677de169-b761-4af3-b54e-3f76ed4466d7)

- **Repeat rate and single purchase**
![螢幕擷取畫面 2024-04-23 093349](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/82e1bcb6-5ac9-49da-a0fa-7e6858d7ba55)



# Data_analysis_Results&Opinion
![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/213387fa-0acc-4c66-a6dc-4b92e6dc7b78)

**A few things can be seen from the data provided:**
- **Date range: from 11/25/2017 to 12/3/2017.**
- **Number of page views (pv): during this period, the number of daily page views ranged from approximately 8.84 million to approximately 12.3 million.**
- **Unique visitors (uv): the number of daily unique visitors increased from approximately 686,000 to approximately 939,000 per day.**
- **Average page views per unique visitor (puv): this figure was relatively stable, fluctuating between 13.1 and 13.7.**

**See data analysis:**
- **User Engagement: The value of puv shows the average number of pages viewed by users during each visit. A number around 13 indicates that users tend to browse multiple pages when visiting Taobao, which - may reflect the attractiveness of the site and user engagement.**
- **User Growth: An increase in the number of unique visitors within a given date range, which may be due to specific marketing campaigns, the holiday shopping season or natural growth.**
- **Traffic Fluctuations: Especially during the first weekend of December (December 2 and 3), pv and uv values increased significantly, which could be due to increased traffic from weekend events or promotions and possibly an early warm-up for the Double-December event.**

![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/518073d8-a92d-4964-9811-f2516ca18328)

**The resulting dataframe retention_rate displays daily retention rates:**
- **Dates: Specific days in late November and early December 2017.**
- **retention_1: The ratio of users who returned the following day to those who visited on the same day. Values close to 1 suggest a higher retention rate.**

**From the results:**

- **On most days, the retention rates are between 0.777 and 0.796, suggesting that about 77.7% to 79.6% of users who visited the site returned the next day.**
- **There is a notable increase on December 1st and 2nd, where the retention rates are much higher (~0.98), possibly indicating a successful marketing campaign or event that drove higher user engagement and return.**
- **The NaN value on December 3rd means there were no same-day visits recorded to compare against, or no data for the following day to establish if users returned.**

![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/afb08ce6-2b47-4538-8044-f6a230993e3d)

**Results Explanation**
**Bounce Users: 88
This indicates that there are 88 users in the dataset who visited the website only once.**

**Total PV (Total Page Views): 89,660,688
It shows that the dataset records a total of 89,660,688 page views.**

**Bounce Rate: 9.81478081006918e-07
The bounce rate is extremely low, nearly zero. This rate indicates that a tiny proportion of all page views were generated by users who visited the website only once.**

**Summary**
**although there are 88 users who only visited once, this number is very small compared to the total page views, resulting in an extremely low bounce rate (approximately 0.000000981%). This suggests that the vast majority of page views come from users who visit the website repeatedly, indicating good user retention and engagement. This can be seen as a positive sign of user involvement and return visits to the website or application.**

![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/2c8a9892-bf41-4726-b395-71643b30c11c)

- **The results show the number of users in each category, with Bronze having the most and Gold the least. This classification indicates varying levels of customer loyalty and engagement, with Diamond Members likely being the most valuable customer group due to their active purchase behaviors.**

- **Conversion Rates Analysis**
![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/46990317-b361-427a-b7c4-97d436f43316)

- **PV TO CAR&FAV Conversion Rate: 9.39%
Industry Standard: For e-commerce websites, the average conversion rate for adding items to the shopping cart typically ranges between 5% to 10%. Therefore, a rate of 9.39% can be considered relatively good, especially as it is near or slightly above the industry average.
Analysis: If this figure is below your industry average or below that of your competitors, measures may need to be taken to improve this ratio. If the figure is already high, the focus may shift to how to convert these potential buyers into actual purchasers.**

- **CAR&FAV TO BUY Conversion Rate: 23.94%
Industry Standard: The conversion rate from cart to purchase typically ranges between 20% to 30%, although this can vary significantly depending on the specific industry and type of product. For example, high-value goods or luxury items might have lower conversion rates compared to inexpensive daily goods.
Analysis: A conversion rate of 23.94% indicates that a considerable proportion of users decide to make a purchase after adding items to their cart or favorites. This rate is quite healthy, showing that once users exhibit purchase intent, they are likely to complete the purchase.**

- **Conclusion
The PV to CAR&FAV conversion rate (9.39%) can be considered at or slightly above the industry average, which is a positive signal indicating effective page design and product attractiveness. The CAR&FAV to BUY conversion rate (23.94%) also performs well, demonstrating a high likelihood of users completing a purchase after adding items to their cart.**

- **Improvement Suggestions
Even though these conversion rates look good, there is always room for improvement. Consider employing A/B testing for different page layouts, enhancing product descriptions, providing more customer reviews, optimizing pricing strategies, and increasing promotional activities to further boost these conversion rates. Additionally, regular analysis of customer behavior and feedback can help identify new areas for improvement, continuously optimizing the user experience and the purchasing process.**

- **Conversion Rate &Bounce Rate**
![image](https://github.com/ryanng9672/Taobao-User-Behavior-Data-Analysis/assets/158177590/bae927b6-ceb7-41dc-8331-fc062e26455a)

- **Optimize the Checkout Process: Since the conversion rates for both cart and favorites to purchase are low, streamlining the checkout process, providing more payment options, and enhancing security might encourage more users to complete their purchases.**

- **Enhance Trust and Credibility: Providing more product details, customer reviews, and clear return policies can help in building trust and encouraging purchases.**

- **Targeted Marketing and Retargeting Campaigns: Implementing targeted marketing strategies to attract the right customers and retargeting campaigns to bring back users who have shown interest could improve conversion rates.**

- **A/B Testing: Conduct A/B testing on various elements of the website to determine what changes can lead to better user engagement and higher conversions.**
  
- **Overall, the data suggests there is significant room for improvement in both engaging users and converting interest into sales. Focusing on enhancing user experience and optimizing the sales funnel could lead to better performance metrics.**

- **Optimize User Experience: Ensure that the checkout process is clear and straightforward to minimize barriers that could lead to user dropout. For users who take time deciding and use the favorites feature, consider providing more detailed product information, user reviews, or enhanced comparison tools.**

- **Enhance User Engagement: For users who frequently use the favorites function, consider re-engaging them with regular reminders about their cart and favorites via email or push notifications, especially for items they have not yet purchased.**

- **Deepen Data Analysis: Investigate the patterns of users who move items from favorites to purchase. Understanding when and why they return from the favorites list to the cart to complete a purchase can help refine marketing strategies and improve conversion rates.**

- **Personalize Recommendations: For users who add items to their favorites, personalized recommendations and promotions might increase conversion rates. This can be achieved by analyzing their browsing and purchase history.**

# WebsiteRelated
- data set link 
https://tianchi.aliyun.com/dataset/649





