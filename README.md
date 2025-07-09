iTRADE Attrition Analysis: Project Documentation
Overview
This Power BI project analyzes 6 months of account activity data for iTRADE to identify patterns of user
attrition (churn), determine key drivers, and develop actionable strategies to reduce churn and improve
customer retention. The report is structured into four main pages: Overview, Deep Dive, Strategy &
Recommendations, and Additional Insights.
1. Data Sources and Modeling
• Primary Dataset: Client-level activity data containing account IDs, balance data (first and last),
monthly balance fluctuations, client segment, active months, and churn flag.
• Modeling Steps:
o Data cleaned and shaped in Power Query.
o Relationships defined on Account_ID and Month.
o Custom calculated tables for derived KPIs.
o Measures created using DAX for all metrics and insights.
o CSV uploaded to Power BI Web Service and merged with existing model.
2. Key Assumptions in Dataset Creation
a. Churn Logic
• A user is considered churned if their last_active_month does not equals the latest available month
in the dataset and they do not appear in later months.
• Absence after the final activity month is assumed to represent silent churn.
b. Lifecycle Binning
• Users are grouped based on their number of active months:
o 1–2 months
o 3–4 months
o 5–6 months
• Drop-off logic compares user counts between bins to estimate churn timing.
c. Client Segmentation
• Segments used: Explorer, Trader, Early — assumed to represent distinct behavioral personas.
• Used in tree visualizations and churn comparison charts.
d. Balance Banding
• Balance ranges were categorized into bands:
o <$5K, $5K–$20K, $20K–$50K, $50K+
• These were used in bar charts, tree diagrams, and risk heatmaps.
e. Revenue Lost Assumption
• Assumed flat 1% fee on last balance to approximate annual revenue.
• Used to calculate churn-driven revenue loss.
f. Cohort Analysis
pg. 1
• Users grouped by first_active_month.
• Drop-off and churn rates analyzed per cohort.
g. Activity Mapping
• Months Active calculated by count of non-zero balance entries.
• Missing months = inactivity; absence in future = churn.
h. Drop-off Measure
• Drop-off calculated as difference in user counts across lifecycle buckets.
• % Drop-off derived to assess client retention effectiveness.
i. Dynamic Slicer Integration
• Filters used for segment, churn, cohort, and balance band.
• Enabled interactive filtering of visuals and KPI cards.
j. UI & Visual Logic
• Red used for churn/high-risk; blue/green for retained users.
• Clean grouping of visuals by topic per page.
• Power BI Web used to publish and link calculated measures across pages.
3. Key Metrics and DAX Logic
a. Churn Definition
• Churn Flag (churn_flag): True if the last_active_month = maximum available month and the
account did not continue in next month.
b. Churn Rate
Churn Rate =
DIVIDE(
CALCULATE(COUNT(client_level[Account_ID]), client_level[churn_flag] = TRUE()),
CALCULATE(COUNT(client_level[Account_ID]))
)
c. Avg Balance Before Churn
AvgBalanceBeforeChurn =
CALCULATE(
AVERAGE(ActivityChangeByMonth[balance]),
FILTER(ActivityChangeByMonth, ActivityChangeByMonth[churn_flag] = TRUE() &&
ActivityChangeByMonth[Month] = ActivityChangeByMonth[last_active_month] - 1)
)
d. Revenue Lost
Revenue_Lost =
SUMX(
ChurnedAccounts,
ChurnedAccounts[last_balance] * 0.01
)
pg. 2
e. Balance Band Column
BalanceBand =
SWITCH(
TRUE(),
balance < 5000, "<5K",
balance < 20000, "5K-20K",
balance < 50000, "20K-50K",
"50K+"
)
4. Dashboard Pages Summary
Page 1: Overview
• Churn %, Avg Balances, Donut Chart, Signup Trend.
Page 2: Deep Dive
• Cohort Signup, Balance Change, % Drop-off by lifecycle.
Page 3: Strategy & Recommendations
• KPI Cards, Drop-off Metrics, 6 Strategy Boxes.
Page 4: Additional Insights
• Segmentation Tree, Balance vs Churn Flag, Waterfall of User Journey.
5. Insights Summary
• Churn clustered in low balance buckets (<$5K).
• Most churn within first 3 months.
• January had the highest churn rate
• Churned users have lower balance volatility on average, which shows low profits can also be a
reason for churn
6. Strategic Recommendations
• Early detection, low-balance retention perks, volatility alerts.
• Watchlists and crypto-oriented engagement (non-trade).
7. UX Enhancements
• Dynamic slicers, clean color-coding, tree/heatmap visuals.
8. Limitations & Future Work
• Revenue loss is approximated.
• No behavioral/session tracking.
• Could embed churn forecast model for automation.
• Potential to incorporate Net Promoter Score.
• Unavailability of Power BI Desktop limited scope of creatively designing the dashboard
pg. 3
pg. 4
