#  Google Play Store App Insights — Power BI Analytics Project

##  Executive Summary

This repository contains an end-to-end business intelligence solution built in **Microsoft Power BI Desktop** analyzing **10,800+ mobile applications** and **64,000+ user reviews** from the Google Play Store.

The goal of this project is to uncover actionable market trends, app monetization strategies, user satisfaction drivers, and sentiment patterns to guide app developers, product managers, and executive leadership in launching and scaling successful Android applications.

---

##  Key Business Findings & KPIs

* **Market Scale**: **9,638 unique applications** analyzed, accounting for **75.3 Billion total installs**.
* **Quality Benchmark**: Overall store average rating is **4.17 / 5.0**, with **65.16%** of all apps maintaining a rating of **4.0 or higher** (76.8% of rated apps).
* **Monetization Landscape**: **92.2% of apps are Free** and capture **98.7% of total installs**, while **7.8% are Paid**.
* **User Engagement**: Free apps receive **26.7x more reviews** (average **234,036 reviews**) compared to Paid apps (**8,759 reviews**).
* **Audience Reach**: The **`Everyone`** content rating dominates the store, representing **81.8% of all apps** (7,886 apps).
* **Category Leaders by Downloads**: **`GAME` (13.3B)**, **`COMMUNICATION` (11.0B)**, **`TOOLS` (7.9B)**, and **`FAMILY` (6.2B)** represent the top install drivers.
* **Ratings vs. Popularity Correlation**: Pearson correlation between Installs and Rating is **+0.04** (near zero), proving that massive popularity does not guarantee high user ratings.
* **Sentiment Intelligence**: Analysis of 37,400+ clean user reviews reveals **64.1% Positive**, **22.1% Negative**, and **13.8% Neutral** sentiment with an average polarity of **+0.18**.

---

##  Dashboard Architecture (3 Interactive Pages Overview)

The interactive report is packaged in [`Report..pbix`](https://github.com/shiva12-cell/App-Insights-Analysis_Power-BI/blob/main/Report/Report..pdf) and structured across three dedicated analytical views:

### 1. Executive Overview Dashboard
* **Top KPI Banner**: Total Apps (`9.64K`), Average Rating (`4.17`), `% Apps Rated 4+` (`65.2%`), Total Installs (`75.3B`).
* **Market Split (Donut Chart)**: Free vs. Paid app proportions.
* **Category Breakdown (Clustered Bar Chart)**: Total installs and app count by store category.
* **Audience Segmentation (Treemap)**: Content rating distribution (`Everyone`, `Teen`, `Mature 17+`, `Everyone 10+`).
* **Maintenance & Lifecycle (Line Chart)**: Frequency and volume of app updates over time, showing an exponential maintenance surge in 2017–2018.
* **Hall-of-Fame Leaderboard (Table)**: Top 5 most reviewed billion-download applications (Facebook, WhatsApp, Instagram, Messenger, Subway Surfers).

### 2. Pricing & Rating Analysis
* **Correlation KPI Card**: Quantitative Pearson correlation measure (`+0.04`).
* **Price vs. Rating (Scatter Plot)**: Analysis of paid applications showing the highest user satisfaction sweet spot between **$0.99 and $4.99** (average rating `4.27`).
* **App Size vs. Installs (Scatter Plot)**: Categorical bubble plot illustrating that both ultra-light utilities (<15 MB) and heavy gaming apps (>40 MB) can achieve 100M+ downloads.
* **Engagement Disparity (Column Chart)**: Comparative review volumes between Free and Paid tiers.
* **Category Rating Leaderboard**: Identifies `EVENTS` (4.44), `EDUCATION` (4.36), and `ART_AND_DESIGN` (4.36) as the highest-satisfaction categories.
* **Genre Hit Density (Treemap)**: Highlights `Tools` (171 apps) and `Action` (127 apps) as producing the highest concentration of apps surpassing 1 Million installs.

### 3. User Sentiment & Feedback Intelligence
* **Sentiment Header Cards**: Total User Reviews (`37.4K`), `% Positive Sentiment` (`64.1%`), and `Median Rating` (`4.3`).
* **Binned Install Analysis (Line Chart)**: Tracks rating evolution across install tiers (`<10k` at 4.04 up to `100M–1B` peaking at 4.39).
* **Genre Benchmarking (Clustered Column Chart)**: Compares Mean vs. Median ratings across major genres to detect negative outlier impact.
* **Sentiment Distribution (Donut Chart)**: Positive, Negative, and Neutral review breakdown.
* **Interactive Reviews Explorer (Table)**: Searchable user feedback matrix that dynamically filters comments when clicking sentiment slices or app categories.


---

##  Strategic Recommendations for App Publishers

1. **Adopt a Freemium Strategy**: With Free apps capturing over 98% of downloads and driving 26.7x higher review velocity, launching free with in-app purchases or subscriptions is the proven path to market dominance.
2. **Optimize Pricing Tiers**: If releasing a paid product, price between **$0.99 and $4.99**. This range maintains the highest customer satisfaction score (4.27) while minimizing buyer friction.
3. **Target File Size by Vertical**: Keep utility and productivity tools under **15 MB** to maximize global install conversions, especially in bandwidth-constrained regions. Gaming titles can expand up to **75–100 MB** provided visual quality justifies the download.
4. **Maintain Continuous Release Cycles**: Over 83% of top-performing apps pushed version updates within the prior 12 months. Routine maintenance directly defends app store search ranking and ratings.

