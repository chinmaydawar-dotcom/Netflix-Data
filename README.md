# Netflix-Data
Strategic, leadership-oriented analysis of Netflix’s content portfolio using data analytics and interactive visualization
Project Overview

This project delivers a strategic, leadership-oriented analysis of Netflix’s content portfolio using data analytics and interactive visualization.

Rather than focusing only on descriptive statistics, the project is designed to uncover how Netflix’s content choices translate into scale, engagement, and long-term strategic advantage. The dashboard enables senior stakeholders to explore trade-offs between content volume, retention drivers, audience maturity, and refresh dependency through interactive KPIs and slicers.

The final output is a Power BI executive dashboard, supported by Python-based exploratory data analysis and AI-assisted insights.
Objectives:
Understand Netflix’s content scale and composition

Identify retention anchors vs scale drivers

Analyze audience maturity concentration

Translate KPIs into actionable leadership insights

Build an interactive, decision-support dashboard
**Tools Used**
🔹 Julius AI

Used for AI-assisted exploratory data analysis

Helped accelerate hypothesis generation and pattern identification

Supported early analytical direction

🔹 Python (Pandas)

Used for data loading, cleaning, and exploratory analysis

Generated analytical summaries used in dashboard design

Handled multi-value fields (countries, genres, durations)

🔹 Power BI

Used to build the interactive executive dashboard

Implemented slicer-driven KPIs and strategic visuals

Designed with leadership decision-making in mind

🔹 ChatGPT

Used as a strategic co-pilot

Assisted in:

Structuring the analytical narrative

Designing KPI logic

Translating analysis into leadership-grade insights

Framing the dashboard story and README documentation
Data Preparation & Exploratory Analysis (Python)
1️⃣ Initial Data Loading & Inspection

Purpose:
Load the Netflix dataset and understand its structure, schema, and missing values.

Key actions:

Read CSV data

Inspect schema and data types

Identify missing values
python: # Load and inspect the Netflix dataset for EDA
import pandas as pd

netflix_df = pd.read_csv('netflix_titles.csv', encoding='utf-8')

print(netflix_df.head())
print(netflix_df.info())
print(netflix_df.isna().sum())
Exploratory Data Analysis (EDA)

Purpose:
Generate foundational insights on content distribution, trends, genres, ratings, and durations.

Analyses performed:

Movie vs TV Show distribution

Year-wise content addition trends

Country-wise content contribution (handling multi-country entries)

Genre popularity analysis

Rating distribution across content types

Duration analysis for Movies and TV Shows
code - # Basic EDA aggregations needed for the questions
import pandas as pd

# 1) Movies vs TV Shows distribution
content_counts = netflix_df['type'].value_counts()

# 2) Content addition trends by year
netflix_df['date_added'] = pd.to_datetime(netflix_df['date_added'])
netflix_df['year_added'] = netflix_df['date_added'].dt.year
add_trend = netflix_df.groupby('year_added')['show_id'].count().dropna()

# 3) Country-wise content contribution
country_split = netflix_df['country'].dropna().str.split(',', expand=False)
country_exploded = netflix_df.loc[netflix_df['country'].notna(), ['show_id']].join(
    country_split.rename('country_list')
).explode('country_list')
country_exploded['country_list'] = country_exploded['country_list'].str.strip()
country_counts = country_exploded['country_list'].value_counts().head(15)

# 4) Genre popularity
genre_split = netflix_df['listed_in'].str.split(',', expand=False)
genre_exploded = netflix_df[['show_id']].join(
    genre_split.rename('genre_list')
).explode('genre_list')
genre_exploded['genre_list'] = genre_exploded['genre_list'].str.strip()
genre_counts = genre_exploded['genre_list'].value_counts().head(15)

# 5) Rating distribution across content types
rating_type = netflix_df.pivot_table(
    index='rating',
    columns='type',
    values='show_id',
    aggfunc='count',
    fill_value=0
)

# 6) Duration analysis
movies = netflix_df[netflix_df['type'] == 'Movie'].copy()
movies['duration_min'] = movies['duration'].str.replace(' min', '', regex=False)
movies['duration_min'] = pd.to_numeric(movies['duration_min'], errors='coerce')

tv = netflix_df[netflix_df['type'] == 'TV Show'].copy()
tv['seasons'] = tv['duration'].str.replace(' Seasons', '', regex=False)
tv['seasons'] = tv['seasons'].str.replace(' Season', '', regex=False)
tv['seasons'] = pd.to_numeric(tv['seasons'], errors='coerce')

movies_duration_summary = movies['duration_min'].describe()
tv_season_summary = tv['seasons'].describe()

print(content_counts)
print(add_trend.tail())
print(country_counts)
print(genre_counts)
print(rating_type)
print(movies_duration_summary)
print(tv_season_summary)
Robust Date Handling & Trend Validation

Purpose:
Ensure reliability when parsing inconsistent date formats.
code - # Convert date_added safely and recompute yearly trends
import pandas as pd

netflix_df['date_added'] = pd.to_datetime(netflix_df['date_added'], errors='coerce')
netflix_df['year_added'] = netflix_df['date_added'].dt.year

add_trend = netflix_df.groupby('year_added')['show_id'].count()

print(add_trend.tail())

**Power BI Dashboard**

Dashboard Structure

Page 0 – Executive Overview

Global slicers (Content Type, Genre, Country, Rating)

KPI cards for scale, retention, maturity, and refresh pressure

Designed as a control tower for leadership

Page 1 & 2 – Analytical Deep-Dives

Genre × Content Type analysis

Audience maturity and engagement patterns

Visual storytelling for retention logic

Final Page – Strategic Insight

KPI-driven leadership signals

Visual-first, action-oriented insights

Focus on trade-offs between scale, engagement, and sustainability

**How to Run the Project**

Python Analysis

Ensure netflix_titles.csv is available

Run the Python scripts for data inspection and EDA

Power BI Dashboard

Open Final Data Assignment.pbix

Use Page 0 slicers to control the entire dashboard

Navigate through analytical and insight pages

**Key Takeaway**

This project transforms raw Netflix content data into a decision-support system, enabling leadership to understand where content investment creates the most strategic leverage.

Netflix’s advantage is not driven by content volume alone, but by how precisely content investment is allocated between scale, engagement, and longevity.


