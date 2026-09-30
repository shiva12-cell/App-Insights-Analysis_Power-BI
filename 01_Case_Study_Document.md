# 📋 Case Study Document: App Insights Unlocked
---

## 1. Background & Context

Imagine you are a Senior Data Analyst working for a mobile technology enterprise specializing in software products for the Android ecosystem. The company has acquired a comprehensive market dataset from the Google Play Store capturing metadata for over 10,800 applications across 33 categories, along with 64,000+ qualitative user reviews.

To maximize market penetration, user retention, and monetization, executive leadership requires an empirical, data-driven analysis to understand what makes an application successful on the Google Play Store.

---

## 2. Problem Statement

Your objective is to ingest, clean, model, and analyze the Google Play Store ecosystem in **Microsoft Power BI** to uncover market patterns, pricing elasticities, sizing thresholds, and user sentiment drivers.

The insights from this analysis will guide:
1. **Product Strategy**: What categories, genres, and feature sizes offer the highest return on development investment?
2. **Monetization Strategy**: How does paid pricing impact user ratings, volume, and customer satisfaction?
3. **Quality & Maintenance**: What is the relationship between version update cadence, install scale, and user sentiment?

---

## 3. Stakeholder Analysis

| Stakeholder Group | Role | Core Business Interest |
|---|---|---|
| **Senior Leadership** | Chief Executive & Strategy Officers | High-level market share, monetization potential, market gaps, risk benchmarks. |
| **Product Managers** | Feature & Roadmap Leads | Category benchmarks, competitive ratings, feature-to-size trade-offs. |
| **App Developers** | Engineering & Architecture Teams | App size optimization (MB limits), Android OS compatibility, update frequency. |
| **Marketing Teams** | User Acquisition & Brand Leads | Content rating targeting (`Everyone` vs. `Teen`), review velocity, store positioning. |
| **External Stakeholders** | End Users & Platform Partners | Age appropriateness, transparency, bug resolution speed, software stability. |

---

## 4. Data Dictionary

### Table 1: `googleplaystore` (Application Metadata)

| Column Name | Data Type | Description | Sample Values |
|---|---|---|---|
| **`App`** | Text | Name of the mobile application | *"Instagram"*, *"Subway Surfers"* |
| **`Category`** | Text | Market category classification | `GAME`, `COMMUNICATION`, `TOOLS` |
| **`Rating`** | Decimal | Average user review score (scale: 1.0 – 5.0) | `4.2`, `4.5`, `null` |
| **`Reviews`** | Integer | Total count of user reviews submitted | `78158306`, `967` |
| **`Size`** | Text $\rightarrow$ Decimal | Installation package file size in Megabytes (MB) | `"19M"` $\rightarrow$ `19.0`, `"512k"` $\rightarrow$ `0.5` |
| **`Installs`** | Integer | Milestone download count | `"1,000,000+"` $\rightarrow$ `1000000` |
| **`Type`** | Text | Commercial licensing model | `Free` or `Paid` |
| **`Price`** | Decimal | Retail purchase price in USD | `"$4.99"` $\rightarrow$ `4.99`, `"0"` $\rightarrow$ `0.00` |
| **`Content Rating`** | Text | Regulatory age appropriateness rating | `Everyone`, `Teen`, `Mature 17+` |
| **`Genres`** | Text | Sub-category genre classification | `Art & Design`, `Action;Pretend Play` |
| **`Last Updated`** | Date | Timestamp of most recent build published to store | `"January 7, 2018"` $\rightarrow$ `2018-01-07` |
| **`Current Ver`** | Text | Application build version identifier | `"1.0.0"`, `"Varies with device"` |
| **`Android Ver`** | Text | Minimum Android operating system required | `"4.0.3 and up"`, `"4.1 and up"` |

### Table 2: `googleplaystore_user_reviews` (Qualitative Sentiment)

| Column Name | Data Type | Description | Scale / Values |
|---|---|---|---|
| **`App`** | Text | Foreign key linking to application entity | *"10 Best Foods for You"* |
| **`Translated_Review`** | Text | Cleaned, English-translated user feedback | User comment string |
| **`Sentiment`** | Text | Classified emotional tone | `Positive`, `Negative`, `Neutral` |
| **`Sentiment_Polarity`** | Decimal | Quantitative positivity/negativity index | `-1.0` (Extremely Negative) to `+1.0` (Extremely Positive) |
| **`Sentiment_Subjectivity`**| Decimal | Objective fact vs. subjective opinion index | `0.0` (Purely Objective) to `1.0` (Purely Subjective) |

---

## 5. Data Cleaning & Preprocessing Specifications

1. **Anomaly Purge (Row 10472)**: Strip corrupted record (*"Life Made WI-Fi Touchscreen Photo Frame"*) where columns were left-shifted (Rating = 19, Category missing).
2. **Deduplication**: Deduplicate application names by sorting by `Reviews` descending and taking unique `App` records (reduces 10,840 rows to 9,638 distinct apps).
3. **Data Type Casting**:
   * Clean `Installs`: Strip `+` and `,`; cast to `Int64`.
   * Clean `Price`: Strip `$` symbol; cast to `Decimal`.
   * Clean `Size`: Parse suffix `M` to $1\text{x}$ and `k`/`K` to $\frac{1}{1024}\text{x}$; convert `"Varies with device"` to `null`.
   * Clean `Rating`: Convert `"NaN"` string artifacts to database `null` / `BLANK()`.
   * Clean `Last Updated`: Parse string date into standard ISO `Date` format (`YYYY-MM-DD`).
4. **User Reviews Cleaning**: Remove records where `Translated_Review` or `Sentiment` is `null` or `"nan"`, filtering 64,295 raw rows down to 37,427 clean qualitative evaluations.

---

## 6. Official Challenge Question Bank

### Basic-Level Questions (10 Questions)
1. What is the average rating of apps in the dataset?
2. How many unique categories of apps are there?
3. What is the distribution of app sizes?
4. How many free vs. paid apps are there?
5. What is the most common content rating for apps?
6. What are the top 5 most installed apps?
7. How many apps have a rating of 4.0 and above?
8. What is the average number of reviews for free vs. paid apps?
9. What is the average app size for each category?
10. How many apps were last updated in 2018?

### Medium-Level Questions (10 Questions)
1. What is the correlation between the number of installs and the app rating?
2. Which app categories have the highest average rating?
3. How does the price of an app affect its average rating?
4. What is the distribution of app ratings across different content ratings?
5. Which genres have the most apps with over 1 million installs?
6. How frequently do apps get updated? Calculate update recency patterns.
7. What is the impact of app size on the number of installs?
8. Which apps have the highest number of reviews, and what are their ratings?
9. How does the content rating distribution differ between free and paid apps?
10. What are the top 5 categories with the most installs?

### Advanced-Level Questions (5 Questions)
1. What are the top 10 apps with the highest ratings, and how do their number of reviews and installs compare?
2. Analyze the trend of app updates over time. Are there noticeable patterns or seasonal trends?
3. How does the average rating of apps change with the number of installs? Create a binned analysis.
4. Perform sentiment analysis on app reviews to determine the common themes in high and low-rated apps.
5. What is the relationship between app genre and user ratings? Compare mean and median ratings.
