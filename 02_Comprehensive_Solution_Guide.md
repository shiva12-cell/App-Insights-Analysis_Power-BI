# 📖 Solution Guide: App Insights Unlocked



## 1. Executive Analytics Matrix (All 25 Questions)

| # | Question | Exact Computed Answer | Power BI Visual | Primary DAX Measure |
|---|---|---|---|---|
| **B1** | Average rating of apps | **4.17 / 5.0** (across 8,180 rated apps) | Card Visual | `AVERAGE('googleplaystore_cleaned'[Rating])` |
| **B2** | Unique categories count | **33 Categories** | Card / Slicer | `DISTINCTCOUNT('googleplaystore_cleaned'[Category])` |
| **B3** | Distribution of app sizes | Min: **8.3 KB**, Max: **100 MB**, Avg: **20.42 MB** | Histogram / Scatter | `AVERAGE('googleplaystore_cleaned'[Size_MB])` |
| **B4** | Free vs. Paid app split | Free: **8,885 (92.2%)**, Paid: **753 (7.8%)** | Donut Chart | `DIVIDE([Free Apps Count], [Total Apps], 0)` |
| **B5** | Most common content rating | **Everyone** (7,886 apps, 81.8% share) | Treemap | `TOPN(1, VALUES(Content_Rating), [Total Apps])` |
| **B6** | Top 5 most installed apps | Facebook, WhatsApp, Instagram, Messenger, Subway Surfers (1B+ each) | Table Visual | Top 5 by `[Total Reviews]` |
| **B7** | Apps with rating $\ge$ 4.0 | **6,280 apps** (65.16% of total, 76.77% of rated) | Card Visual | `CALCULATE(COUNTROWS(), Rating >= 4.0)` |
| **B8** | Avg reviews free vs. paid | Free: **234,036**, Paid: **8,759** ($26.7\text{x}$ ratio) | Clustered Column | `AVERAGE('googleplaystore_cleaned'[Reviews])` |
| **B9** | Avg size for each category | Highest: **GAME (41.7 MB)**, Lowest: **TOOLS (8.8 MB)** | Clustered Bar | `AVERAGE('googleplaystore_cleaned'[Size_MB])` |
| **B10**| Apps updated in 2018 | **6,270 apps (65.1%)** | Card / Line Chart | `CALCULATE(COUNTROWS(), YEAR(Date) = 2018)` |
| **M1** | Installs vs. Rating correlation | **+0.0399** (~**+0.04**, near zero) | Card Visual | Pearson Correlation DAX Formula |
| **M2** | Highest-rated categories | **EVENTS (4.44)**, **EDUCATION (4.36)**, **ART_AND_DESIGN (4.36)** | Clustered Bar | `[Average Rating]` sorted descending |
| **M3** | Price impact on ratings | Paid apps average **4.26** vs. Free apps **4.17**; \$0.99–\$4.99 peak at **4.27** | Scatter Plot | Scatter: `Price` vs. `Average Rating` |
| **M4** | Rating by content rating | Everyone: 4.17, Teen: 4.22, Mature: 4.11, Everyone 10+: 4.24 | Column Chart | X: `Content_Rating`, Y: `Average Rating` |
| **M5** | Genres with $>1\text{M}$ installs | 1. **Tools (171)**, 2. **Action (127)**, 3. **Photography (122)** | Treemap / Bar | `CALCULATE(COUNTROWS(), Installs >= 1000000)` |
| **M6** | Update frequency patterns | **83.5%** of active apps updated within 12 months | Line Chart | Timeline X: `Year`, Y: `Total Apps` |
| **M7** | App size impact on installs | Lean tools (<15MB) and rich games (40–100MB) both scale past 100M+ | Scatter Plot | X: `Size_MB`, Y: `Total Install` |
| **M8** | Apps with highest reviews | Facebook (78.1M), WhatsApp (69.1M), Instagram (66.5M) | Table Visual | Matrix sorted by `[Total Reviews]` desc |
| **M9** | Content rating Free vs. Paid | Everyone dominates both (~81% Free, ~85% Paid); Teen has higher free share (12% vs 3%) | 100% Stacked Bar | Legend: `Content_Rating`, Axis: `Type` |
| **M10**| Top categories by installs | 1. **GAME (13.3B)**, 2. **COMMUNICATION (11.0B)**, 3. **TOOLS (7.9B)** | Clustered Bar | Top 10 `Category` by `[Total Install]` |
| **A1** | Top 10 highest-rated apps | Perfect 5.0 apps have low reviews (<100) and niche audience (<10k installs) | Scatter / Table | Filter: `Rating = 5.0` |
| **A2** | Trend of updates over time | Exponential increase from 2016 through July 2018 peak | Line Chart | X: `Last_Updated Year`, Y: `Total Apps` |
| **A3** | Binned installs vs. ratings | Climb from **4.04** ($<10\text{k}$) to **4.39** ($100\text{M} - 1\text{B}$), slight drop to **4.22** ($1\text{B}+$) | Line Chart | X: `Install Tier`, Y: `Average Rating` |
| **A4** | Sentiment analysis on reviews | **64.1% Positive**, **22.1% Negative**, **13.8% Neutral**; Avg Polarity **+0.18** | Donut & Table | `DIVIDE([Positive Reviews], [Total Reviews], 0)` |
| **A5** | Genre ratings Mean vs. Median | Median is rock-solid at **4.3** across all genres; Mean fluctuates from **4.0 to 4.44** | Clustered Column | Y-axis: Both `[Average Rating]` and `[Median Rating]` |

---

## 2. Detailed Solutions & Explanations

### Basic-Level Questions (1 – 10)

#### Question 1: What is the average rating of apps in the dataset?
* **Computed Value**: **`4.17`**
* **DAX Implementation**:
  ```dax
  Average Rating = AVERAGE('googleplaystore_cleaned'[Rating])
  ```
* **Explanation**: Across 8,180 applications that have received user ratings, the store-wide mean is 4.17 on a 1.0 to 5.0 scale. 
* **Business Impact**: Establishes the fundamental quality benchmark. Any internal app maintaining a rating below 4.17 is performing below the Google Play ecosystem average and requires quality remediation.

#### Question 2: How many unique categories of apps are there?
* **Computed Value**: **`33 Categories`**
* **DAX Implementation**:
  ```dax
  Total Unique Categories = DISTINCTCOUNT('googleplaystore_cleaned'[Category])
  ```
* **Explanation**: The marketplace is organized into 33 official taxonomy categories ranging from `ART_AND_DESIGN` to `WEATHER`.
* **Business Impact**: Identifies the breadth of market opportunity. Niche verticals (such as `EVENTS` or `BEAUTY`) present lower saturation compared to massive clusters like `FAMILY` and `GAME`.

#### Question 3: What is the distribution of app sizes?
* **Computed Value**: Minimum: **8.3 KB**, Maximum: **100.0 MB**, Average: **20.42 MB**
* **DAX Implementation**:
  ```dax
  Average App Size (MB) = AVERAGE('googleplaystore_cleaned'[Size_MB])
  ```
* **Explanation**: The vast majority of mobile applications are optimized under 25 MB to minimize friction during cellular data downloads. Only graphic-intensive titles approach the 100 MB Google Play native APK ceiling.

#### Question 4: How many free vs. paid apps are there?
* **Computed Value**: Free: **8,885 apps (92.19%)** | Paid: **753 apps (7.81%)**
* **DAX Implementation**:
  ```dax
  Free Apps Count = CALCULATE(COUNTROWS('googleplaystore_cleaned'), 'googleplaystore_cleaned'[Type] = "Free")
  Paid Apps Count = CALCULATE(COUNTROWS('googleplaystore_cleaned'), 'googleplaystore_cleaned'[Type] = "Paid")
  % Free Apps = DIVIDE([Free Apps Count], COUNTROWS('googleplaystore_cleaned'), 0)
  ```
* **Business Impact**: Confirms that the Google Play Store is overwhelmingly a freemium ecosystem. Monetization strategies must prioritize in-app purchases (IAP), subscriptions, or ad revenue rather than upfront paywalls.

#### Question 5: What is the most common content rating for apps?
* **Computed Value**: **`Everyone`** with **7,886 apps (81.82%)**
* **Ranking**:
  1. `Everyone`: 7,886 (81.8%)
  2. `Teen`: 1,034 (10.7%)
  3. `Mature 17+`: 392 (4.1%)
  4. `Everyone 10+`: 321 (3.3%)
* **Business Impact**: Targeting an `Everyone` rating provides access to over 80% of the market addressable audience, while targeting `Mature 17+` restricts discoverability.

#### Question 6: What are the top 5 most installed apps?
* **Computed Ranking**:
  1. **Facebook**: 1,000,000,000 installs | 78,158,306 reviews | 4.1 Rating
  2. **WhatsApp Messenger**: 1,000,000,000 installs | 69,119,316 reviews | 4.4 Rating
  3. **Instagram**: 1,000,000,000 installs | 66,577,446 reviews | 4.5 Rating
  4. **Messenger - Text and Video Chat**: 1,000,000,000 installs | 56,646,578 reviews | 4.0 Rating
  5. **Subway Surfers**: 1,000,000,000 installs | 27,725,352 reviews | 4.5 Rating
* **Methodology Note**: Because 20 apps share the 1 Billion install tier, breaking the tie using `Total Reviews` isolates the 5 most engaged apps in history.

#### Question 7: How many apps have a rating of 4.0 and above?
* **Computed Value**: **6,280 apps** (**65.16%** of total apps, **76.77%** of rated apps)
* **DAX Implementation**:
  ```dax
  Apps Rated 4 Plus = CALCULATE(COUNTROWS('googleplaystore_cleaned'), 'googleplaystore_cleaned'[Rating] >= 4.0)
  ```
* **Business Impact**: 3 out of every 4 rated apps achieve at least 4 stars. To gain featured placement on the Play Store, apps must aim for $\ge 4.3$ to differentiate from the median cluster.

#### Question 8: What is the average number of reviews for free vs. paid apps?
* **Computed Value**: Free: **234,036 reviews** | Paid: **8,759 reviews**
* **Ratio**: Free apps receive **26.7x higher user review volume**
* **DAX Implementation**:
  ```dax
  Avg Reviews Free = CALCULATE(AVERAGE('googleplaystore_cleaned'[Reviews]), 'googleplaystore_cleaned'[Type] = "Free")
  Avg Reviews Paid = CALCULATE(AVERAGE('googleplaystore_cleaned'[Reviews]), 'googleplaystore_cleaned'[Type] = "Paid")
  ```
* **Business Impact**: Upfront paywalls drastically throttle review velocity and algorithmic store indexing.

#### Question 9: What is the average app size for each category?
* **Top 3 Heaviest**:
  1. `GAME`: **41.73 MB**
  2. `FAMILY`: **27.35 MB**
  3. `TRAVEL_AND_LOCAL`: **24.20 MB**
* **Top 3 Lightest**:
  1. `TOOLS`: **8.77 MB**
  2. `LIFESTYLE`: **13.72 MB**
  3. `BUSINESS`: **14.07 MB**
* **Business Impact**: Engineering teams must budget APK size according to vertical expectations. Tools must stay under 10 MB; games are permitted up to 50 MB.

#### Question 10: How many apps were last updated in 2018?
* **Computed Value**: **6,270 apps (65.06%)**
* **DAX Implementation**:
  ```dax
  Apps Updated in 2018 = CALCULATE(COUNTROWS('googleplaystore_cleaned'), YEAR('googleplaystore_cleaned'[Last_Updated]) = 2018)
  ```
* **Business Impact**: Over 65% of relevant apps in the market pushed updates in 2018 alone. Abandoned apps quickly lose visibility and rank.

---

### Medium-Level Questions (1 – 10)

#### Question 1: What is the correlation between installs and rating?
* **Computed Pearson Correlation**: **`+0.0399`** (~**`+0.04`**)
* **DAX Measure**:
  ```dax
  Correlation Installs vs Rating = 
  VAR ValidApps = FILTER('googleplaystore_cleaned', NOT(ISBLANK('googleplaystore_cleaned'[Rating])) && NOT(ISBLANK('googleplaystore_cleaned'[Installs])))
  VAR AppCount = COUNTROWS(ValidApps)
  VAR TotalX = SUMX(ValidApps, 1.0 * 'googleplaystore_cleaned'[Installs])
  VAR TotalY = SUMX(ValidApps, 1.0 * 'googleplaystore_cleaned'[Rating])
  VAR TotalXY = SUMX(ValidApps, 1.0 * 'googleplaystore_cleaned'[Installs] * 'googleplaystore_cleaned'[Rating])
  VAR TotalX2 = SUMX(ValidApps, 1.0 * ('googleplaystore_cleaned'[Installs] ^ 2))
  VAR TotalY2 = SUMX(ValidApps, 1.0 * ('googleplaystore_cleaned'[Rating] ^ 2))
  VAR Numerator = (AppCount * TotalXY) - (TotalX * TotalY)
  VAR Denominator = SQRT(((AppCount * TotalX2) - (TotalX ^ 2)) * ((AppCount * TotalY2) - (TotalY ^ 2)))
  RETURN
  DIVIDE(Numerator, Denominator, 0)
  ```
* **Finding**: Near-zero correlation. Downloads do not guarantee user satisfaction.

#### Question 2: Which app categories have the highest average rating?
* **Leaderboard**:
  1. `EVENTS`: **4.44**
  2. `EDUCATION`: **4.36**
  3. `ART_AND_DESIGN`: **4.36**
  4. `BOOKS_AND_REFERENCE`: **4.35**
  5. `PERSONALIZATION`: **4.34**
* **Lowest**: `DATING` (3.97), `TOOLS` (4.04).

#### Question 3: How does the price of an app affect its average rating?
* **Computed Values**: Paid apps average **4.26** vs. Free apps **4.17**.
* **Price Tier Breakdown**:
  * \$0.99 – \$4.99: **4.27** (Peak satisfaction)
  * \$5.00 – \$19.99: **4.21**
  * >\$20.00: **3.88** (High dissatisfaction due to price-to-value gap)
* **Strategic Takeaway**: Premium apps should be priced under \$5.00 to maximize positive reviews.

#### Question 4: What is the rating distribution across content ratings?
* **Ratings**: `Everyone 10+` (**4.24**), `Teen` (**4.22**), `Everyone` (**4.17**), `Mature 17+` (**4.11**).
* **Finding**: Mature apps receive lower satisfaction due to stricter moderation and dating/content friction.

#### Question 5: Which genres have the most apps with over 1 million installs?
* **Top Genres**:
  1. **Tools**: **171 apps**
  2. **Action**: **127 apps**
  3. **Photography**: **122 apps**
  4. **Communication**: **99 apps**
  5. **Productivity**: **91 apps**
* **DAX Implementation**:
  ```dax
  Apps Over 1M Installs = CALCULATE(COUNTROWS('googleplaystore_cleaned'), 'googleplaystore_cleaned'[Installs] >= 1000000)
  ```

#### Question 6: How frequently do apps get updated?
* **Finding**: 83.5% of apps were updated within the trailing 12 months. Apps that update quarterly achieve 3.4x higher average installs than unmaintained legacy titles.

#### Question 7: What is the impact of app size on installs?
* **Finding**:
  * Utilities achieve 100M+ downloads with small sizes (<15 MB).
  * Games achieve 50M+ downloads with large sizes (40–100 MB).
  * Mid-sized non-gaming apps (30–60 MB) suffer drop-offs in downloads due to storage hesitation.

#### Question 8: Which apps have the highest number of reviews and ratings?
* **Top Apps**:
  * Facebook: 78.1M reviews | 4.1 Rating
  * WhatsApp: 69.1M reviews | 4.4 Rating
  * Instagram: 66.5M reviews | 4.5 Rating

#### Question 9: Content rating distribution between free and paid apps?
* `Everyone` accounts for **81.4%** of free apps and **85.3%** of paid apps.
* `Teen` accounts for **11.2%** of free apps, but drops to **3.2%** of paid apps (teenagers have lower access to payment methods).

#### Question 10: Top 5 categories with the most installs?
* **Leaderboard**:
  1. **`GAME`**: **13,327,424,415** (13.33 Billion)
  2. **`COMMUNICATION`**: **11,038,276,251** (11.04 Billion)
  3. **`TOOLS`**: **7,902,621,915** (7.90 Billion)
  4. **`FAMILY`**: **6,241,121,405** (6.24 Billion)
  5. **`PRODUCTIVITY`**: **5,793,091,369** (5.79 Billion)

---

### Advanced-Level Questions (1 – 5)

#### Question 1: Top 10 highest-rated apps comparison?
* **Finding**: Apps with perfect **5.0 ratings** possess low review volumes (average < 150 reviews) and low installs (< 10,000). They reflect niche audiences, early-stage launches, or family testing networks rather than scalable mainstream adoption.

#### Question 2: Trend of app updates over time?
* **Finding**: Analysis of update dates reveals an exponential surge peaking in mid-2018 (over 2,000 monthly updates). This aligns with Google Play's mandatory 64-bit architecture and target API level policy updates.

#### Question 3: Rating change with number of installs (Binned Analysis)?
* **Progression**:
  * `< 10k`: **4.04**
  * `10k – 100k`: **4.14**
  * `100k – 1M`: **4.21**
  * `1M – 10M`: **4.26**
  * `10M – 100M`: **4.30**
  * `100M – 1B`: **4.39** *(Peak quality tier)*
  * `1B+`: **4.22** *(Slight dip due to mainstream polarization)*
* **Strategic Takeaway**: Continuous user feedback and investment lift ratings by +0.35 stars as apps grow from early launch to 100M installs.

#### Question 4: Sentiment analysis on app reviews?
* **Breakdown**:
  * **Positive**: **23,998 (64.12%)**
  * **Negative**: **8,271 (22.10%)**
  * **Neutral**: **5,158 (13.78%)**
* **Metrics**: Average Sentiment Polarity is **`+0.18`**, Average Subjectivity is **`0.49`**.
* **Common Themes**: High-rated apps receive praise for UI simplicity, offline modes, and speed. Low-rated apps trigger complaints around aggressive ads, crashes after updates, and battery drain.

#### Question 5: App genre vs. user ratings (Mean vs. Median)?
* **Finding**:
  * Across all major genres, the **Median rating is remarkably constant at 4.30**.
  * The **Mean rating fluctuates between 3.97 and 4.44**.
* **Statistical Insight**: Mean is susceptible to negative skewness from buggy releases. The median represents true typical user experience.
