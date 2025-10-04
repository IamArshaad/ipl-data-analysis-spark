<h1 align="center">IPL Data Analysis using PySpark & Databricks</h1>

<p align="center">

  <img src="https://github.com/user-attachments/assets/f72780bc-2772-4462-b83f-a9429857efd4" alt="IPL Logo" width="150"/>
</p>

<p align="center">
  <b>Data Engineering | Big Data Analytics | PySpark | AWS S3 | Databricks | SQL | Visualization</b>
</p>

---


### Overview

This project performs an **end-to-end analysis of Indian Premier League (IPL)** data using **PySpark** on **Databricks**, with raw datasets stored in **AWS S3**.  

The primary goal was **to learn PySpark for distributed data processing, transformations, and analytics**, while performing exploratory and SQL-based analysis on IPL match data.  

While the insights are exploratory, this project demonstrates practical skills in **data ingestion, cleaning, transformation, Spark SQL, and visualization** - forming a strong foundation in data engineering and analytics.

---

### Tech Stack & Tools

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="48" alt="Python"/>
  <img src="https://img.icons8.com/color/48/apache-spark.png" alt="apache-spark"/>
  <img src="https://github.com/user-attachments/assets/f69d8ed6-b2e4-4bc4-8137-3c1322991642" width="48" alt="Databricks"/>
  <img src="https://img.icons8.com/color/48/amazon-web-services.png" alt="amazon-web-services"/>
  <img src="https://img.icons8.com/color/48/git.png" alt="git"/>
  <img src="https://img.icons8.com/ios-filled/50/github.png" alt="github"/>
</p>




| Component | Purpose |
|------------|----------|
| **AWS S3** | Cloud storage for raw IPL CSV files |
| **Databricks Community Edition** | Spark environment for development and compute |
| **PySpark** | Data ingestion, cleaning, transformation |
| **Spark SQL** | Querying transformed data |
| **Python (Matplotlib, Seaborn, Pandas)** | Visualization and insights |
| **Git & GitHub** | Version control and documentation |

---

### Dataset Information

**Source:** [Cricsheet.org](http://cricsheet.org/)  
Original data was available in **YAML** format and converted to **CSV** (via R, SQL, SSIS).

**Files Used:**
| File | Description |
|------|--------------|
| `Ball_By_Ball.csv` | Ball-by-ball details for all matches |
| `Match.csv` | Match-level details (venue, result, etc.) |
| `Player.csv` | Player details |
| `Player_Match.csv` | Player performance and participation per match |
| `Team.csv` | Team information |

**Seasons Covered:** IPL 2008–2017

---

### Project Architecture

<p align="center">
  <img src="https://github.com/user-attachments/assets/c793bf1a-8df9-4324-832b-11194fe14b47" width="700" alt="Project Architecture"/>
</p>



---

### Workflow

#### 1️. Data Ingestion
- Read CSVs directly from **AWS S3** using `s3a://` path.
- Defined **PySpark StructType schemas** for all datasets.
- Loaded all datasets into Spark DataFrames.

#### 2️. Data Cleaning & Transformation
- Removed duplicates and nulls.
- Renamed and reformatted columns.
- Created new calculated columns.
- Joined related tables (e.g., player ↔ match ↔ team).
- Applied filtering and aggregation using PySpark functions.

#### 3️. SQL-based Analysis
- Registered temporary views and ran **Spark SQL** for insights.
- Example analyses:
  - Top players by average runs in winning matches.
  - Team performance trends per season.
  - Venue-wise match distribution.

#### 4️. Visualization
- Converted Spark DataFrames to Pandas.
- Used **Matplotlib** and **Seaborn** for bar charts, pie charts, and trend visualizations.

---

### Example Query

```python
average_runs_in_a_win = spark.sql("""
SELECT p.player_name,
       AVG(b.runs_scored) AS avg_runs_in_wins,
       COUNT(*) AS innings_played
FROM ball_by_ball b
JOIN player_match pm ON b.match_id = pm.match_id AND b.striker = pm.player_id
JOIN player p ON pm.player_id = p.player_id
JOIN match m ON pm.match_id = m.match_id
WHERE m.match_winner = pm.player_team
GROUP BY p.player_name
ORDER BY avg_runs_in_wins DESC
""")
display(average_runs_in_a_win)
