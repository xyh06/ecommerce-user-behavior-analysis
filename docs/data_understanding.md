# Data Understanding

## 1. Dataset Overview

The dataset contains 50,000 customers with aggregated behavioral and transaction features.
Each row represents a single customer rather than an individual event or order.

Therefore, this dataset is a **user-level aggregated table**, not a transactional log table.

---

## 2. Business Interpretation

The data likely comes from an e-commerce platform user profile system.

Possible data pipeline:
Tracking Logs → Session Aggregation → Customer Feature Engineering → Analysis Table

So this dataset is suitable for:

* churn analysis
* user segmentation
* retention analysis
* behavior pattern discovery

Not suitable for:

* detailed funnel step timing
* path analysis
* event sequence mining

---

## 3. Target Variable

`Churned` = 1 indicates the user has left the platform.
This will be treated as the business outcome variable.

Business goal:
Identify behavior patterns that lead to churn and propose retention strategies.

---

## 4. Key Feature Groups

### User Attributes

Age, Gender, Country, City, Membership_Years

### Engagement Behavior

Login_Frequency, Session_Duration_Avg, Pages_Per_Session,
Wishlist_Items, Social_Media_Engagement_Score, Mobile_App_Usage

### Purchase Behavior

Total_Purchases, Average_Order_Value, Lifetime_Value,
Days_Since_Last_Purchase

### Risk Indicators

Cart_Abandonment_Rate, Returns_Rate, Discount_Usage_Rate

### Support Interaction

Customer_Service_Calls, Email_Open_Rate, Product_Reviews_Written

# 数据理解（Data Understanding）

## 1. 缺失值机制分析（Missing Mechanism Analysis）

本数据集的缺失值并非随机产生。  
通过对比流失用户（Churned=1）与未流失用户（Churned=0）的缺失比例发现：

多个行为相关字段（如 `Session_Duration_Avg`、`Email_Open_Rate`）在流失用户中具有更高缺失率。

这表明缺失值很可能代表“用户未发生该行为”，而非系统未记录数据。例如：

- `Session_Duration_Avg` 缺失可能表示用户近期未产生会话
- `Email_Open_Rate` 缺失可能表示用户未打开营销邮件
- `Mobile_App_Usage` 缺失可能表示用户未使用APP

因此，本数据集缺失值属于 **行为缺失（Behavioral Missingness）**，  
缺失本身具有业务含义，可作为预测用户流失的重要信号，而不应简单填充或删除。

后续特征工程将保留缺失信息，并构建“是否发生行为”的指示变量（indicator features）。

---

## English Summary

Missing values in this dataset are not random.  
By comparing churned and non-churned users, several behavioral features show higher missing rates among churned users.

This indicates missing values represent **absence of behavior** rather than data recording failure.

Therefore, missingness itself contains predictive information and will be preserved as behavioral indicators instead of being imputed or removed.

## Insight 1: Engagement Decline Precedes Churn

Analysis shows that churned users exhibit significantly lower engagement levels across multiple behavioral metrics:

Login frequency (-27.9%)

Email open rate (-30.6%)

Social engagement (-27.3%)

Mobile app usage (-22.4%)

This suggests that churn is not sudden but preceded by gradual disengagement.

## Insight 2: Operational Friction Signals

Churned users demonstrate:

Higher customer service calls (+33.1%)

Higher cart abandonment rate (+18.4%)

Higher return rate (+11.3%)

This indicates potential dissatisfaction or purchase friction.

## Insight 3: Recency is the Strongest Signal

Days since last purchase is 37% higher for churned users, making recency the strongest observable indicator.

## Insight 4: Churn is Driven by Engagement Collapse Rather Than Value

Correlation analysis shows churn has minimal relationship with customer value metrics:

Lifetime Value (-0.01)

Membership Years (~0)

Payment Diversity (~0)

However, churn strongly correlates with behavioral activity decline:

Pages per session (-0.23)

Session duration (-0.23)

Email open rate (-0.22)

Mobile app usage (-0.22)

This indicates churn is not caused by low-value users leaving, but by previously normal users gradually disengaging.

## Insight 5: Service Friction is the Earliest Observable Trigger

Customer service calls (0.29) and cart abandonment (0.28) show the highest positive correlations with churn.

This suggests a causal sequence:

Service friction → hesitation → purchase stop → churn

Churn therefore appears to be experience-driven rather than demand-driven.

## Insight 6: Churn Follows a Progressive Cooling Process

Churn rate increases monotonically with time since last purchase:

≤7 days: 22%

7–15 days: 25%

15–27 days: 27%

27–48 days: 30%

≥48 days: 40%

This indicates churn is not an instantaneous event but a gradual disengagement process.

We define a behavioral death threshold around 30–45 days of inactivity, after which churn probability accelerates significantly.

This provides a clear intervention window for retention strategies.

## Insight 7: Churn Is Caused by Disengagement, Not Purchase Failure

Churn rate shows a strong monotonic relationship with login frequency:

Lowest activity users: 47% churn

Highest activity users: 19% churn

This indicates customers churn because they stop visiting the platform, not because they fail to convert during visits.

Retention strategy should prioritize re-engagement (notifications, reminders, content), rather than discounts.

## Insight 8: Customer Service Calls Reveal a Failure Point

Churn increases dramatically after 5 support contacts:

≤5 contacts → ~17% churn

5 contacts → ~40–48% churn

Customer service interaction is not a retention action but a late-stage failure signal.

Users contact support repeatedly before leaving, indicating unresolved issues rather than recoverable dissatisfaction.

This suggests companies should trigger escalation policies instead of standard support once the threshold is crossed.

## Insight 9: High-Value Customers Are More Likely to Churn

Churn rate increases monotonically with Average Order Value, reaching over 40% in the highest spending group.

This contradicts the common assumption that premium users are more loyal.

Combined with high support contact frequency and declining activity before churn, this suggests:

High-value customers leave not due to price sensitivity but due to unmet expectations and unresolved service issues.

Retention strategy should prioritize experience recovery instead of discount incentives.

## Final Business Rule: Early Churn Warning Segment

By combining behavioral thresholds discovered in the analysis:

Customer_Service_Calls ≥ 5

Days_Since_Last_Purchase ≥ 30

We can identify a high-risk user segment.

Churn rate comparison:

Normal users: 24.0%

High-risk users: 45.97%

This simple rule nearly doubles churn probability and can be used as an operational trigger before deploying predictive models.

Suggested action:
Immediately escalate these users to retention workflow (support recovery / service intervention) rather than discount campaigns.

