# Provide-Insights-for-a-Strategic-Merger-in-the-OTT-Domain
Reliance and Disney have launched JioHotstar, merging JioCinema and Disney+ Hotstar content libraries. Existing subscribers of both platforms can retain their plans but must use JioHotstar. HBO, NBCUniversal Peacock, and Paramount, all under one roof.

#!/usr/bin/env python
# coding: utf-8

# # Strategic Merger in the OTT Domain

# 1. Load Required Libraries

# In[1]:


import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


# 2. Load the Datasets

# In[4]:


# Load LioCinema datasets
lio_contents = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\liocinema_content.csv")
lio_subscribers = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\liocinema_subscriber.csv")
lio_consumption = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\liocinema_content_consumption.csv")


# In[5]:


# Load Jotstar datasets
jot_contents = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\jotstar_content.csv")
jot_subscribers = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\jotstar_subscriber.csv")
jot_consumption = pd.read_csv(r"C:\Users\HP\Downloads\Codebasic.io\RPC14_Input_For_Participants\RPC14_Input_For_Participants\datasets\jotstar_content_consumption.csv")


# 3. Check Data Overview

# In[6]:


# Display first few rows of each dataset
print("Lio_content\n")
print(lio_contents.head())

print("Lio_subscribers\n")
print(lio_subscribers.head())

print("lio_content_consumption\n")
print(lio_consumption.head())


# In[7]:


print("jot_content \n")
print(jot_contents.head())
print("jot_subscribers \n")
print(jot_subscribers.head())
print("jot_content_consumption \n")
print(jot_consumption.head())


# In[8]:


# Check data types & missing values
print(lio_contents.info())
print(jot_contents.info())


# In[9]:


print(lio_subscribers.info())
print(jot_subscribers.info())


# In[10]:


print(lio_consumption.info())
print(jot_consumption.info())


# In[11]:


# Summary statistics
print(lio_contents.describe())
print(jot_contents.describe())


# # Key Metrics Calculation
# Total Users & Growth Trends

# In[12]:


# Total users
total_lio_users = lio_subscribers['user_id'].nunique()
total_jot_users = jot_subscribers['user_id'].nunique()

print(f"Total LioCinema Users: {total_lio_users}")
print(f"Total Jotstar Users: {total_jot_users}")


# In[13]:


# Monthly user growth
lio_subscribers['subscription_month'] = pd.to_datetime(lio_subscribers['subscription_date']).dt.to_period('M')
jot_subscribers['subscription_month'] = pd.to_datetime(jot_subscribers['subscription_date']).dt.to_period('M')

monthly_growth_lio = lio_subscribers.groupby('subscription_month')['user_id'].nunique()
monthly_growth_jot = jot_subscribers.groupby('subscription_month')['user_id'].nunique()

plt.figure(figsize=(10,5))
monthly_growth_lio.plot(label="LioCinema", marker='o')
monthly_growth_jot.plot(label="Jotstar", marker='s')
plt.title("Monthly User Growth Trend")
plt.xlabel("Month")
plt.ylabel("New Users")
plt.legend()
plt.show()


# In[14]:


# Total number of contents
total_lio_contents = lio_contents['content_id'].nunique()
total_jot_contents = jot_contents['content_id'].nunique()

print(f"Total LioCinema Contents: {total_lio_contents}")
print(f"Total Jotstar Contents: {total_jot_contents}")


# In[15]:


# Content type distribution
plt.figure(figsize=(10, 4))
sns.countplot(data=lio_contents, x='content_type', label="LioCinema", color="blue", alpha=0.6)
sns.countplot(data=jot_contents, x='content_type', label="Jotstar", color="red", alpha=0.6)
plt.legend()
plt.title("Content Type Distribution")
plt.show()


# In[16]:


# Language distribution
plt.figure(figsize=(10, 5))
sns.histplot(lio_contents['language'], label="LioCinema", color="blue", alpha=0.6)
sns.histplot(jot_contents['language'], label="Jotstar", color="red", alpha=0.6)
plt.legend()
plt.title("Content Language Distribution")
plt.xticks(rotation=45)
plt.show()


# Active vs. Inactive Users

# In[17]:


# Active & Inactive Users
lio_active_users = lio_subscribers['last_active_date'].isna().sum()
jot_active_users = jot_subscribers['last_active_date'].isna().sum()

lio_inactive_users = lio_subscribers['last_active_date'].notna().sum()
jot_inactive_users = jot_subscribers['last_active_date'].notna().sum()

print(f"LioCinema - Active Users: {lio_active_users}, Inactive Users: {lio_inactive_users}")
print(f"Jotstar - Active Users: {jot_active_users}, Inactive Users: {jot_inactive_users}")


# In[18]:


# Percentage
lio_inactive_rate = (lio_inactive_users / total_lio_users) * 100
jot_inactive_rate = (jot_inactive_users / total_jot_users) * 100

print(f"LioCinema Inactive Rate: {lio_inactive_rate:.2f}%")
print(f"Jotstar Inactive Rate: {jot_inactive_rate:.2f}%")


# Watch Time Analysis

# In[19]:


# Convert minutes to hours
lio_consumption['total_watch_time_hrs'] = lio_consumption['total_watch_time_mins'] / 60
jot_consumption['total_watch_time_hrs'] = jot_consumption['total_watch_time_mins'] / 60

avg_watch_time_lio = lio_consumption['total_watch_time_hrs'].mean()
avg_watch_time_jot = jot_consumption['total_watch_time_hrs'].mean()

print(f"Average Watch Time (LioCinema): {avg_watch_time_lio:.2f} hrs")
print(f"Average Watch Time (Jotstar): {avg_watch_time_jot:.2f} hrs")


# In[20]:


plt.figure(figsize=(10, 5))

# Box plot for LioCinema
sns.boxplot(x="device_type", y="total_watch_time_hrs", data=lio_consumption, color="blue")
sns.boxplot(x="device_type", y="total_watch_time_hrs", data=jot_consumption, color="red")

plt.title("Watch Time Distribution by Device Type")
plt.xlabel("Device Type")
plt.ylabel("Total Watch Time (hrs)")

# Adding legend manually
import matplotlib.patches as mpatches
blue_patch = mpatches.Patch(color='blue', label='LioCinema')
red_patch = mpatches.Patch(color='red', label='Jotstar')
plt.legend(handles=[blue_patch, red_patch])

plt.show()


# 8. Upgrade & Downgrade Analysis

# In[21]:


# Upgraded Users
lio_upgraded_users = lio_subscribers[lio_subscribers['new_subscription_plan'] > lio_subscribers['subscription_plan']].shape[0]
jot_upgraded_users = jot_subscribers[jot_subscribers['new_subscription_plan'] > jot_subscribers['subscription_plan']].shape[0]

lio_upgrade_rate = (lio_upgraded_users / total_lio_users) * 100
jot_upgrade_rate = (jot_upgraded_users / total_jot_users) * 100

print(f"LioCinema Upgrade Rate: {lio_upgrade_rate:.2f}%")
print(f"Jotstar Upgrade Rate: {jot_upgrade_rate:.2f}%")


# In[22]:


# Downgraded Users
lio_downgraded_users = lio_subscribers[lio_subscribers['new_subscription_plan'] < lio_subscribers['subscription_plan']].shape[0]
jot_downgraded_users = jot_subscribers[jot_subscribers['new_subscription_plan'] < jot_subscribers['subscription_plan']].shape[0]

lio_downgrade_rate = (lio_downgraded_users / total_lio_users) * 100
jot_downgrade_rate = (jot_downgraded_users / total_jot_users) * 100

print(f"LioCinema Downgrade Rate: {lio_downgrade_rate:.2f}%")
print(f"Jotstar Downgrade Rate: {jot_downgrade_rate:.2f}%")


# 9. Revenue Analysis

# In[23]:


# Subscription pricing assumption
pricing = {'Free': 0, 'Basic': 199, 'VIP': 299, 'Premium': 499}

# Calculate revenue
lio_subscribers['monthly_revenue'] = lio_subscribers['subscription_plan'].map(pricing)
jot_subscribers['monthly_revenue'] = jot_subscribers['subscription_plan'].map(pricing)

total_revenue_lio = lio_subscribers['monthly_revenue'].sum()
total_revenue_jot = jot_subscribers['monthly_revenue'].sum()

print(f"Total Revenue (LioCinema): ₹{total_revenue_lio}")
print(f"Total Revenue (Jotstar): ₹{total_revenue_jot}")


# In[24]:


# Convert date columns to datetime
date_cols = ['subscription_date', 'last_active_date', 'plan_change_date']
for col in date_cols:
    lio_subscribers[col] = pd.to_datetime(lio_subscribers[col])
    jot_subscribers[col] = pd.to_datetime(jot_subscribers[col])


# In[25]:


# --- 1. Total Content Items ---
lio_total_content = lio_contents.shape[0]
jot_total_content = jot_contents.shape[0]


# In[26]:


# --- 2. Total Users ---
lio_total_users = lio_subscribers.shape[0]
jot_total_users = jot_subscribers.shape[0]


# In[27]:


# --- 3 & 4. Paid Users & Paid Users % ---
lio_paid_users = lio_subscribers[lio_subscribers['subscription_plan'] != 'Free'].shape[0]
jot_paid_users = jot_subscribers[jot_subscribers['subscription_plan'] != 'Free'].shape[0]

lio_paid_users_pct = (lio_paid_users / lio_total_users) * 100
jot_paid_users_pct = (jot_paid_users / jot_total_users) * 100


# In[28]:


# --- 5 & 6. Active vs. Inactive Users ---
lio_active_users = lio_subscribers[lio_subscribers['last_active_date'].isna()].shape[0]
jot_active_users = jot_subscribers[jot_subscribers['last_active_date'].isna()].shape[0]

lio_inactive_users = lio_total_users - lio_active_users
jot_inactive_users = jot_total_users - jot_active_users


# In[29]:


# --- 7 & 8. Inactive Rate & Active Rate ---
lio_inactive_rate = (lio_inactive_users / lio_total_users) * 100
jot_inactive_rate = (jot_inactive_users / jot_total_users) * 100

lio_active_rate = 100 - lio_inactive_rate
jot_active_rate = 100 - jot_inactive_rate


# In[30]:


# --- 9 & 10. Upgraded Users & Upgrade Rate ---
lio_upgrades = lio_subscribers[lio_subscribers['new_subscription_plan'] > lio_subscribers['subscription_plan']].shape[0]
jot_upgrades = jot_subscribers[jot_subscribers['new_subscription_plan'] > jot_subscribers['subscription_plan']].shape[0]

lio_upgrade_rate = (lio_upgrades / lio_total_users) * 100
jot_upgrade_rate = (jot_upgrades / jot_total_users) * 100


# In[31]:


# --- 11 & 12. Downgraded Users & Downgrade Rate ---
lio_downgrades = lio_subscribers[lio_subscribers['new_subscription_plan'] < lio_subscribers['subscription_plan']].shape[0]
jot_downgrades = jot_subscribers[jot_subscribers['new_subscription_plan'] < jot_subscribers['subscription_plan']].shape[0]

lio_downgrade_rate = (lio_downgrades / lio_total_users) * 100
jot_downgrade_rate = (jot_downgrades / jot_total_users) * 100


# In[32]:


# --- 13 & 14. Total & Average Watch Time (Hours) ---
lio_total_watch_time = lio_consumption['total_watch_time_mins'].sum() / 60  # Convert mins to hours
jot_total_watch_time = jot_consumption['total_watch_time_mins'].sum() / 60

lio_avg_watch_time = lio_total_watch_time / lio_total_users
jot_avg_watch_time = jot_total_watch_time / jot_total_users


# In[33]:


# --- 15. Monthly User Growth Rate (%) ---
lio_subscribers['month'] = lio_subscribers['subscription_date'].dt.to_period('M')
jot_subscribers['month'] = jot_subscribers['subscription_date'].dt.to_period('M')

lio_growth = lio_subscribers.groupby('month').size().pct_change().fillna(0) * 100
jot_growth = jot_subscribers.groupby('month').size().pct_change().fillna(0) * 100


# In[34]:


# --- 16. Upgrade / Downgrade Comparison ---
upgrade_downgrade_df = pd.DataFrame({
    "Metric": ["Upgraded Users", "Downgraded Users"],
    "LioCinema": [lio_upgrades, lio_downgrades],
    "Jotstar": [jot_upgrades, jot_downgrades]
})


# In[35]:


# --- Visualization ---

# Total users comparison
plt.figure(figsize=(8, 5))
sns.barplot(x=["LioCinema", "Jotstar"], y=[lio_total_users, jot_total_users], palette="Blues_r")
plt.title("Total Users Comparison")
plt.ylabel("Total Users")
plt.show()


# In[36]:


# Active vs Inactive Users
plt.figure(figsize=(10, 5))
sns.barplot(x=["Active", "Inactive"], y=[lio_active_users, lio_inactive_users], label="LioCinema", color="blue")
sns.barplot(x=["Active", "Inactive"], y=[jot_active_users, jot_inactive_users], label="Jotstar", color="red")
plt.legend()
plt.title("Active vs. Inactive Users")
plt.show()


# In[37]:


# Watch Time Comparison
plt.figure(figsize=(10, 5))
sns.barplot(x=["LioCinema", "Jotstar"], y=[lio_avg_watch_time, jot_avg_watch_time], palette="coolwarm")
plt.title("Average Watch Time (Hrs)")
plt.ylabel("Avg Watch Time (hrs)")
plt.show()


# In[38]:


# Upgrade vs Downgrade
upgrade_downgrade_df.set_index("Metric").plot(kind="bar", figsize=(8, 5), colormap="viridis")
plt.title("Upgrade vs Downgrade Comparison")
plt.ylabel("Users")
plt.show()


# In[39]:


# Growth Trends
plt.figure(figsize=(12, 5))
lio_growth.plot(label="LioCinema", marker="o")
jot_growth.plot(label="Jotstar", marker="s", linestyle="dashed")
plt.legend()
plt.title("Monthly User Growth Rate (%)")
plt.xlabel("Month")
plt.ylabel("Growth Rate (%)")
plt.xticks(rotation=45)
plt.show()


# In[40]:


# --- Summary ---
summary = {
    "LioCinema": {
        "Total Content": lio_total_content,
        "Total Users": lio_total_users,
        "Paid Users %": round(lio_paid_users_pct, 2),
        "Inactive Rate %": round(lio_inactive_rate, 2),
        "Upgrade Rate %": round(lio_upgrade_rate, 2),
        "Downgrade Rate %": round(lio_downgrade_rate, 2),
        "Total Watch Time (hrs)": round(lio_total_watch_time, 2),
        "Avg Watch Time (hrs)": round(lio_avg_watch_time, 2)
    },
    "Jotstar": {
        "Total Content": jot_total_content,
        "Total Users": jot_total_users,
        "Paid Users %": round(jot_paid_users_pct, 2),
        "Inactive Rate %": round(jot_inactive_rate, 2),
        "Upgrade Rate %": round(jot_upgrade_rate, 2),
        "Downgrade Rate %": round(jot_downgrade_rate, 2),
        "Total Watch Time (hrs)": round(jot_total_watch_time, 2),
        "Avg Watch Time (hrs)": round(jot_avg_watch_time, 2)
    }
}
# Convert to DataFrame for better visualization
summary_df = pd.DataFrame(summary)
print(summary_df)


# # Primary_And_Secondary_Analysis
# Questions from the available data (Primary)

# 1. Total Users & Growth Trends 
# 
# ● What is the total number of users for LioCinema and Jotstar, and how do they 
# compare in terms of growth trends (January–November 2024)? 

# In[41]:


# Monthly User Growth Rate
lio_subscribers['month'] = lio_subscribers['subscription_date'].dt.to_period('M')
jot_subscribers['month'] = jot_subscribers['subscription_date'].dt.to_period('M')

lio_growth = lio_subscribers.groupby('month').size()
jot_growth = jot_subscribers.groupby('month').size()

plt.figure(figsize=(12, 5))
lio_growth.plot(label="LioCinema", marker="o")
jot_growth.plot(label="Jotstar", marker="s", linestyle="dashed")
plt.legend()
plt.title("Monthly User Growth (Jan–Nov 2024)")
plt.xlabel("Month")
plt.ylabel("New Users")
plt.xticks(rotation=45)
plt.show()


# 2. Content Library Comparison 
# 
# ● What is the total number of contents available on LioCinema vs. Jotstar? How do 
# they differ in terms of language and content type? 

# In[42]:


# Content Type Distribution
lio_content_type = lio_contents['content_type'].value_counts()
jot_content_type = jot_contents['content_type'].value_counts()

plt.figure(figsize=(10, 5))
lio_content_type.plot(kind='bar', alpha=0.7, label="LioCinema", color="blue")
jot_content_type.plot(kind='bar', alpha=0.7, label="Jotstar", color="red")
plt.title("Content Type Distribution")
plt.ylabel("Count")
plt.legend()
plt.show()


# 3. User Demographics 
# 
# ● What is the distribution of users by age group, city tier, and subscription plan for each 
# platform?

# In[43]:


# Age Group Distribution
plt.figure(figsize=(8, 5))
sns.countplot(data=lio_subscribers, x="age_group", color="blue", label="LioCinema")
sns.countplot(data=jot_subscribers, x="age_group", color="red", label="Jotstar", alpha=0.7)
plt.legend()
plt.title("User Distribution by Age Group")
plt.show()


# 4. Active vs. Inactive Users 
# 
# ● What percentage of LioCinema and Jotstar users are active vs. inactive? How do 
# these rates vary by age group and subscription plan? 

# In[44]:


# Active vs Inactive Users
labels = ["Active", "Inactive"]
lio_counts = [lio_active_users, lio_inactive_users]
jot_counts = [jot_active_users, jot_inactive_users]

fig, ax = plt.subplots(1, 2, figsize=(10, 5))
ax[0].pie(lio_counts, labels=labels, autopct="%1.1f%%", colors=["blue", "gray"])
ax[0].set_title("LioCinema")
ax[1].pie(jot_counts, labels=labels, autopct="%1.1f%%", colors=["red", "gray"])
ax[1].set_title("Jotstar")
plt.show()


# 5. Watch Time Analysis 
# 
# ● What is the average watch time for LioCinema vs. Jotstar during the analysis period? 
# How do these compare by city tier and device type?

# In[45]:


plt.figure(figsize=(10, 5))
sns.set_palette(["blue", "red"])  # Define colors for both plots
sns.boxplot(x="device_type", y="total_watch_time_mins", data=lio_consumption)
sns.boxplot(x="device_type", y="total_watch_time_mins", data=jot_consumption)
plt.title("Watch Time Distribution by Device Type")
plt.show()


# In[46]:


plt.figure(figsize=(10, 5))
sns.boxplot(x="device_type", y="total_watch_time_mins", data=lio_consumption, color="blue")
box = sns.boxplot(x="device_type", y="total_watch_time_mins", data=jot_consumption, color="red")

# Adjust alpha (transparency) using matplotlib patches
for patch in box.patches:
    patch.set_alpha(0.6)

plt.title("Watch Time Distribution by Device Type")
plt.show()


# # Revenue Analysis
# ● Assume the following monthly subscription prices, calculate the total revenue generated by both platforms (LioCinema and Jotstar) for the analysis period (January to November 2024).
# 
# The calculation should consider:
# 
# ❖ Subscribers count under each plan.
# 
# ❖ Active duration of subscribers on their respective plans.
# 
# ❖ Upgrades and downgrades during the period, ensuring revenue reflects the time spent under each plan.

# In[47]:


# Define subscription prices for each plan
lio_price_map = {'Free': 0, 'Basic': 5, 'Premium': 10}
jot_price_map = {'Free': 0, 'VIP': 7, 'Premium': 12}

# Calculate revenue for LioCinema
lio_revenue = (lio_subscribers['subscription_plan'].map(lio_price_map) * lio_subscribers.shape[0]).sum()

# Calculate revenue for Jotstar
jot_revenue = (jot_subscribers['subscription_plan'].map(jot_price_map) * jot_subscribers.shape[0]).sum()

print(f"LioCinema Revenue: ${lio_revenue}")
print(f"Jotstar Revenue: ${jot_revenue}")


# # Further analysis & recommendations: 
# 
# 1. What strategies can the merged platform implement to increase engagement among 
# inactive users and convert them into active users? 
# 
# 2. What type of brand campaigns should the merged platform launch to establish itself 
# as the go-to OTT platform in India? 
# 
# 3. How should the merged platform price its subscription plans to compete effectively 
# while maintaining profitability? 
# 
# 4. How can the platform leverage partnerships with telecom companies to expand its 
# subscriber base? 
# 
# 5. What role can AI and machine learning play in personalizing the user experience and 
# improving content discovery? 
# 
# 6. Who should be the brand ambassador for the newly merged OTT platform 
# (LioCinema-Jotstar) to effectively represent its identity and attract a diverse 
# audience? 

# # 1. Increasing Engagement Among Inactive Users
#    Personalized Recommendations – Use AI-driven recommendations based on past viewing history.
# 
#   Push Notifications & Emails – Notify inactive users about trending shows, personalized picks, or expiring content.
# 
#   Gamification & Rewards – Introduce loyalty points or badges for completing shows, referring friends, or engaging with content.
# 
#   Exclusive Content Previews – Offer sneak peeks or early access to upcoming releases for inactive users.
# 
#   Flexible Subscription Models – Provide pay-per-view options or cheaper ad-supported plans for hesitant users.
# 
# # 2. Brand Campaigns for Establishing Market Leadership
#  Localized Content Focus – Create regional ad campaigns featuring actors from different states to tap into diverse audiences.
# 
#   Social Media & Influencer Marketing – Partner with top Indian influencers to showcase exclusive content.
# 
#   "OTT of the Masses" Positioning – Campaign that highlights family-friendly + youth-centric + regional content.
# 
#   Mega Giveaways & Challenges – Conduct watch-based contests where users win free subscriptions or meet their favorite stars.
# 
# # 3. Pricing Strategy for Competitive Edge & Profitability
#      Freemium Model with Ads – Keep a free ad-supported tier with limited content to bring users into the ecosystem.
# 
#      Dynamic Pricing Strategy – Offer lower pricing in Tier 2 & Tier 3 cities to capture price-sensitive audiences.
# 
#      Bundled Plans – Include family subscriptions with multiple screens at a discount.
# 
#      Annual Subscription Discounts – Push yearly plans with savings to ensure long-term retention.
# 
# # 4. Expanding via Telecom Partnerships
#       Bundled with Mobile Data Plans – Offer free subscriptions with Jio, Airtel, Vi, and BSNL recharge packs.
# 
#       Data-Driven Promotions – Leverage telecom user insights to target OTT ads to heavy mobile data users.
# 
#       5G Content Partnership – Provide ultra-HD streaming benefits exclusively on 5G plans to push premium subscriptions.
# 
# # 5. Role of AI & ML in Personalization & Discovery
#       Smart Content Recommendations – Netflix-style personalized playlists based on user behavior.
# 
#       AI-Powered Trailers – Auto-generate personalized previews based on viewing habits.
# 
#       Voice & Chat-Based 

# In[ ]:





# In[ ]:




