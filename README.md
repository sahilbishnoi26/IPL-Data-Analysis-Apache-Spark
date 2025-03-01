# IPL-Data-Pipeline-Analysis-Apache-Spark
A complete IPL data analysis pipeline using Apache Spark, Databricks, and AWS S3. Includes data ingestion, transformation, SQL analytics, and visualization.


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
SELECT P.player_name, M.season_year, SUM(B.runs_scored) AS total_runs
FROM ball_by_ball B
JOIN match M ON B.match_id = M.match_id
JOIN player_match PM ON M.match_id = PM.match_id AND B.striker = PM.player_id
JOIN player P ON PM.player_id = P.player_id
GROUP BY P.player_name, M.season_year
ORDER BY M.season_year, total_runs DESC;
```

### 2️⃣ **Most Economical Bowlers in Powerplay**

```sql
SELECT P.player_name, ROUND(SUM(B.runs_scored) / COUNT(B.ball_id), 2) AS economy_rate
FROM ball_by_ball B
JOIN player_match PM ON B.match_id = PM.match_id AND B.bowler = PM.player_id
JOIN player P ON PM.player_id = P.player_id
WHERE B.over_id < 6
GROUP BY P.player_name
ORDER BY economy_rate ASC
LIMIT 10;
```

### 3️⃣ **Impact of Toss on Match Outcomes**

```sql
SELECT M.toss_winner, M.match_winner, COUNT(*) AS match_count
FROM match M
GROUP BY M.toss_winner, M.match_winner;
```

📌 **Future Improvements:**

- **Real-time processing** using Spark Streaming.
- **Advanced Machine Learning models** for performance predictions.
- **Integration with cloud services** (GCP BigQuery, AWS Redshift, Snowflake).

