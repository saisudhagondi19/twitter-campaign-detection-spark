# Uncovering Coordinated Twitter Campaigns

Detects artificial inflation of Twitter trend popularity using PySpark and AWS — analyzed 300,000+ tweets across 10 trends to identify bot-driven coordinated campaigns.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=flat&logo=apachespark&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws&logoColor=white)
![Amazon EMR](https://img.shields.io/badge/AWS_EMR-FF9900?style=flat&logo=amazonaws&logoColor=white)
![Amazon Redshift](https://img.shields.io/badge/Redshift-8C4FFF?style=flat&logo=amazonredshift&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon_S3-569A31?style=flat&logo=amazons3&logoColor=white)
![QuickSight](https://img.shields.io/badge/QuickSight-FF9900?style=flat&logo=amazonaws&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## Overview

Social media trends can be manipulated. This project builds an end-to-end data pipeline to detect coordinated campaigns artificially inflating Twitter trend popularity — using behavioral signals like duplicate tweet rates, retweet-to-like ratios, and inter-tweet timing patterns.

**10 Twitter trends analyzed. Datasets ranging into GBs per trend.**

---

## Architecture

```
Twitter API (snscrape / TwitterSearchScraper)
          │
          ▼
  Amazon S3  ←── Raw tweet data (JSON format)
          │
          ▼
  AWS EMR + PySpark
  Data cleaning · Feature engineering · Anomaly detection
          │
          ▼
  Amazon S3  ←── Processed data (Parquet format)
          │
          ▼
  Amazon Redshift  ←── Structured data warehouse
          │
          ▼
  AWS QuickSight  ←── Interactive dashboard
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Data collection | snscrape, sntwitter, TwitterSearchScraper |
| Storage | Amazon S3 (JSON → Parquet) |
| Processing | PySpark on AWS EMR |
| Data warehouse | Amazon Redshift |
| Visualization | AWS QuickSight |
| Language | Python |

---

## Key Findings

### Trend 1 — #advancehbdmaheshbabu (302,595 tweets)

| Metric | Value | Signal |
|---|---|---|
| Total tweets analyzed | 302,595 | — |
| Likes < Retweets rate | **83%** | 🚩 Coordinated retweet behavior |
| Duplicate tweets | **38,719 (13%)** | 🚩 Copy-paste amplification |
| Distinct users | 9,921 | — |
| Avg retweets per account | 12 | 🚩 High per-account activity |
| Avg time gap between tweets | 3,230 seconds | 🚩 Irregular burst pattern |
| Quote tweets | 12,923 | — |
| Replies | 23,121 | — |
| Common users across trends | **4.08%** | Low organic cross-trend overlap |

> **Conclusion:** The combination of 83% retweet dominance, 13% duplicate rate, and irregular timing strongly indicates coordinated artificial inflation.

---

### Trend 2 — #Gobackmodi vs #Welcomemodi (Indian General Election, Feb 2019)

| Metric | #Gobackmodi (Anti) | #Welcomemodi (Pro) |
|---|---|---|
| Total tweets | 57,202 | 32,493 |
| Total retweets | 188,825 | 75,053 |
| Total likes | 289,639 | 114,410 |
| Retweets > Likes rate | 18% | 18% |
| Duplicate tweet rate | **19%** | **40%** 🚩 |
| Distinct users | 14,162 | 5,992 |
| Avg retweets per account | 4 | 3 |
| Avg time gap (seconds) | 2,248 | **455** 🚩 |
| Quote tweets | 17,658 | 7,490 |
| Replies | 24,039 | 10,014 |
| Common users in trends | 2.86% | 2.86% |

> **Conclusion:** #Welcomemodi shows a 40% duplicate rate and an average inter-tweet gap of only 455 seconds — hallmarks of coordinated burst activity. Copy-paste behavior was directly observed in the content analysis.

---

## Dataset

Tweets collected via Twitter API using `snscrape` and `TwitterSearchScraper` in JSON format. Each tweet contains 28 fields including:

- `tweet.id`, `tweet.content`, `tweet.date`
- `tweet.likeCount`, `tweet.retweetCount`, `tweet.replyCount`, `tweet.quoteCount`
- `tweet.username`, `tweet.source`
- `tweet.hashtags`, `tweet.mentionedUsers`
- `tweet.vibe` (sentiment score)
- `tweet.viewCount`, `tweet.conversationId`

---

## Data Pipeline

### 1. Collection
Twitter data collected via `TwitterSearchScraper` using hashtag-based search queries and dumped to S3 in JSON format.

### 2. Cleaning & Preprocessing (PySpark on EMR)
- Dropped irrelevant columns (`card`, `cashtags`, `coordinates`, etc.)
- Cast `date` to timestamp
- Filled missing numeric values with `0`, empty strings with `None`
- Normalized text: lowercase, stripped whitespace, removed duplicate spaces
- Stored cleaned output in **Parquet format** on S3 (columnar, query-optimized)

### 3. Analysis (PySpark)
- Duplicate tweet detection via content hashing
- Retweet-to-like ratio analysis per user
- Inter-tweet timing gap calculation
- Cross-trend common user analysis
- Engagement anomaly scoring

### 4. Warehousing & Visualization
- Structured data loaded into **Amazon Redshift**
- **AWS QuickSight** connected to Redshift for interactive dashboards
- Dashboard metrics: duplicate rate, engagement ratio, timing patterns, user overlap

---

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/saisudhagondi19/twitter-campaign-detection-spark.git
cd twitter-campaign-detection-spark
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Collect tweets**
```bash
python scraper.py --hashtag advancehbdmaheshbabu --limit 300000 --output s3://your-bucket/raw/
```

**4. Run PySpark job on EMR**
```bash
spark-submit --master yarn src/analysis.py \
  --input s3://your-bucket/raw/ \
  --output s3://your-bucket/processed/
```

**5. Load to Redshift**
```bash
python load_redshift.py --source s3://your-bucket/processed/
```

Configure AWS credentials as environment variables — never hardcode them:
```bash
export AWS_REGION=us-east-1
export S3_BUCKET=your-bucket-name
export REDSHIFT_HOST=your-cluster.redshift.amazonaws.com
export REDSHIFT_DB=twitterdb
```

---

## Project Structure

```
twitter-campaign-detection-spark/
├── src/
│   ├── scraper.py           # Tweet collection via snscrape
│   ├── analysis.py          # PySpark cleaning + anomaly detection
│   └── load_redshift.py     # Load Parquet → Redshift
├── notebooks/
│   └── eda.ipynb            # Exploratory analysis
├── requirements.txt
└── README.md
```

---

## Requirements

```
pyspark
snscrape
boto3
pandas
psycopg2-binary
```

---

## Coordinated Campaign Signals (Detection Logic)

A trend is flagged as potentially artificial if it meets **2 or more** of these thresholds:

| Signal | Threshold | Why It Matters |
|---|---|---|
| Duplicate tweet rate | > 10% | Copy-paste amplification by bots |
| Retweets > Likes rate | > 75% | Bots retweet but rarely like |
| Avg inter-tweet gap | < 500 sec | Burst posting pattern |
| Avg retweets per account | > 10 | Single accounts over-amplifying |
| Common users across trends | < 5% | Siloed fake accounts |

---

## Conclusions

- Coordinated campaigns use **copy-paste retweet bursts** — 13–40% duplicate rates detected
- Bot accounts show **high retweet, low like** behavior — 83% retweet dominance in #advancehbdmaheshbabu
- Pro-Modi campaign showed more aggressive coordination — 40% duplicates vs 19% for anti-Modi
- Only ~3–4% of users were common across trends, suggesting purpose-built siloed bot accounts
- Timing gaps as low as 455 seconds indicate scripted, automated posting

---

## Future Work

- Real-time streaming detection using Kafka instead of batch processing
- ML-based anomaly detection (graph neural networks for bot network identification)
- Sentiment analysis using BERT for deeper content understanding
- Graph database integration (Neo4j) to map bot network connections
- Scale to full Twitter Firehose for broader trend coverage

---

## License

MIT License — see [LICENSE](LICENSE) for details.
