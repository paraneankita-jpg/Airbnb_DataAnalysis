# 🏠 Airbnb Pricing & Availability Analysis

## ❓ Business Question
What factors are associated with Airbnb listing prices, and which room types, neighbourhoods, and time periods show the strongest signs of demand versus supply imbalance?

## 📊 Dataset
- **Source:** Airbnb listings dataset (`Airbnb_dataset.csv.gz`) — New York City listings
- **Size:** 102,599 rows × 26 columns (raw); 98,840 rows × 25 columns after cleaning
- **Description:** Listing-level data including price, service fee, room type, neighbourhood/neighbourhood group, host details, minimum nights, number of reviews, reviews per month, availability (days/365), and review dates.

## 🛠️ Tools Used
- 🐍 Python (pandas, numpy) — data cleaning and aggregation
- 📈 Python (matplotlib, seaborn) — visualization
- 📓 Jupyter Notebook — analysis environment

## 🔑 Key Findings
1. Median listing price is **~$624** (mean ~$625) — a wide market with substantial price dispersion.
2. **Private rooms and entire homes/apartments** dominate listing supply; these two room types account for the bulk of the 98,840 cleaned listings.
3. **Staten Island** has the highest average availability (~195 days/365) but far fewer average reviews (~35) than the busier boroughs — a supply-heavy, lower-demand pattern. **Manhattan**, by contrast, has the lowest average availability (~134 days) despite lower average reviews than Queens/Bronx, suggesting tighter turnover in that market.
4. Room type visibly affects price distribution (confirmed via boxplot), though the notebook doesn't yet quantify this with a statistical test (e.g. ANOVA) — noted as a limitation below.

## ✅ Recommendations
- Hosts in high-availability, lower-demand areas (e.g. Staten Island) should reconsider pricing or listing presentation to improve booking conversion, since open availability there isn't translating into review/booking activity at the same rate as denser boroughs.
- Prioritize competitive pricing analysis specifically for the two dominant room types (private room, entire home/apt), since that's where most supply — and therefore most price competition — is concentrated.
- Investigate the review-activity time series further before drawing conclusions on seasonality; the current line chart shows month-over-month variation but hasn't been decomposed into trend vs. seasonal effects.

## 📁 Files
- `Airbnb_Analysis.ipynb` — full notebook: data loading, cleaning, EDA, visualizations, and KPI summary
- `Airbnb_dataset.csv.gz` — raw source data (not included in repo if size/licensing restricts it — add a data-access note if so)

## 🔬 Methodology
The raw dataset (102,599 rows) was cleaned by parsing `last review` to datetime, filling missing `reviews per month` with 0, dropping the sparse `license` and `house_rules` columns, and dropping rows with missing values in core fields (name, host info, location, price, service fee, minimum nights, listing counts, availability). Price and service fee fields were stripped of currency symbols and cast to float. Rows with `availability 365` greater than 365 (a logical impossibility) were treated as invalid and excluded from availability-specific analysis. Duplicate rows were removed, leaving 98,840 listings for EDA.

Analysis consisted of a correlation heatmap across numeric fields, distribution and boxplot views of price, count plots for room type and neighbourhood group, a price-by-room-type boxplot, a time series of review volume by last-review month, and a group-level demand/availability comparison across neighbourhood groups.

**⚠️ Known limitations:**
- The neighbourhood group aggregation contains at least one unmerged data entry error (a lowercase, misspelled `"brookln"` category distinct from `"Brooklyn"`) that skews that row's averages and should be fixed via string normalization before the findings above are treated as final.
- No statistical significance testing (e.g. ANOVA, correlation p-values) was performed — visual patterns are described, not confirmed.
- No SQL or dashboard/BI layer is part of this repository; the entire pipeline is Python/Jupyter only.
