# IPL-Data-Pipeline-Analysis-Apache-Spark
IPL data analysis using Apache Spark, Databricks, and AWS S3. Includes data ingestion, transformation, SQL analytics, and visualization.


# IPL Data Analysis using Apache Spark & Databricks

## 📌 Introduction

End-to-end data processing workflow for analyzing IPL (Indian Premier League) cricket data using Apache Spark and Databricks. The workflow includes:

- **Uploading IPL data to Amazon S3**
- **Using Databricks as the compute environment**
- **Transforming data with Apache Spark**
- **Performing SQL-based analysis**
- **Generating insights through visualizations**

![alt text](https://github.com/sahilbishnoi26/IPL-Data-Pipeline-Analysis-Apache-Spark/blob/main/img1.png)

## 📁 Architecture Diagram

The project follows a structured pipeline:

1. **Amazon S3**: Stores raw IPL dataset files.
2. **Databricks Workspace**: Provides a managed environment for Apache Spark execution.
3. **Apache Spark (PySpark)**: Handles data ingestion, transformation, and aggregation.
4. **SQL Queries**: Used for advanced data analysis.
5. **Visualization (Matplotlib & Seaborn)**: Presents insights in graphical format.

---

## 🏗 Project Workflow

### 1️⃣ **Data Ingestion**

- IPL dataset is uploaded to **Amazon S3**
- Data is read into **Apache Spark**
- CSV files are loaded with defined schemas to maintain data consistency.

### 2️⃣ **Data Processing with PySpark**

- Schema definition ensures appropriate data types for each column.
- Data transformation includes:
  - Filtering out `wides` and `no-balls`.
  - Computing `total and average runs` per match and innings.
  - Using **Window Functions** to calculate running totals.
  - Identifying `high impact balls` based on wickets and runs.

### 3️⃣ **SQL-based Analysis**

- SQL queries are written using Spark SQL for insights such as:
  - **Top-scoring batsmen per season**
  - **Most economical bowlers in the powerplay**
  - **Impact of winning the toss on match outcomes**
  - **Average runs contribution of players in winning matches**

### 4️⃣ **Visualization & Insights**

- **Matplotlib & Seaborn** used for graphical representation:
  - **Economical bowlers analysis**
  - **Toss impact on match outcomes**
  - **Runs scored in different venues**
  - **Dismissal types distribution**

---

## 🛠️ Technologies Used

- **Amazon S3** (Object Storage)
- **Databricks** (Cloud-based Apache Spark environment)
- **Apache Spark (PySpark)** (Big Data processing)
- **SQL (Spark SQL)** (Data analysis and querying)
- **Matplotlib & Seaborn** (Data Visualization)
- **Pandas** (Data Handling)

---

## 📂 Dataset Information

The dataset contains multiple CSV files:

1. **Ball-by-ball data** (`ball_by_ball.csv`)
2. **Match-level data** (`match.csv`)
3. **Player information** (`player.csv`)
4. **Player match statistics** (`player_match.csv`)
5. **Team details** (`team.csv`)

---

## 📊 Sample Insights & Queries

### 1️⃣ **Top-Scoring Batsmen per Season**

```sql
SELECT 
p.player_name,
m.season_year,
SUM(b.runs_scored) AS total_runs 
FROM ball_by_ball b
JOIN match m ON b.match_id = m.match_id   
JOIN player_match pm ON m.match_id = pm.match_id AND b.striker = pm.player_id     
JOIN player p ON p.player_id = pm.player_id
GROUP BY p.player_name, m.season_year
ORDER BY m.season_year, total_runs DESC
```

### 2️⃣ **Most Economical Bowlers in Powerplay**

```sql
SELECT 
p.player_name, 
AVG(b.runs_scored) AS avg_runs_per_ball, 
COUNT(b.bowler_wicket) AS total_wickets
FROM ball_by_ball b
JOIN player_match pm ON b.match_id = pm.match_id AND b.bowler = pm.player_id
JOIN player p ON pm.player_id = p.player_id
WHERE b.over_id <= 6
GROUP BY p.player_name
HAVING COUNT(*) >= 1
ORDER BY avg_runs_per_ball, total_wickets DESC
```

### 3️⃣ **Impact of Toss on Match Outcomes**

```sql
SELECT m.match_id, m.toss_winner, m.toss_name, m.match_winner,
       CASE WHEN m.toss_winner = m.match_winner THEN 'Won' ELSE 'Lost' END AS match_outcome
FROM match m
WHERE m.toss_name IS NOT NULL
ORDER BY m.match_id
```

📌 **Future Improvements:**

- **Real-time processing** using Spark Streaming.
- **Advanced Machine Learning models** for performance predictions.
- **Integration with cloud services** (GCP BigQuery, AWS Redshift, Snowflake).

