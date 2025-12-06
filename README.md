# 📌 **README.md — YT-Insight: Intelligent YouTube Comment Analytics**

## 🚀 Overview

**YT-Insight** is an end-to-end YouTube analytics pipeline designed for content creators and data engineers.
It collects YouTube comments at scale, analyzes them using Spark and ML models, and produces rich insights including:

* Sentiment classification
* Emotion detection
* Topic modeling
* Suggested AI replies
* Engagement analysis
* Creator-focused insights
* Comment quality & actionability scoring

The system is organized into **5 clean modules**, each responsible for a specific part of the pipeline.
All modules run directly inside **Google Colab**, using your **Google Drive for storage**, requiring no deployment or GPU (optional).

---

# 🧩 **System Architecture**

```
YouTube URL
   ↓
Module 1 — Scraping (YouTube API)
   ↓ Comments.parquet + Metadata.json
Module 2 — Spark Analytics (Topic Modeling)
   ↓ Topics.json + Stats.parquet
Module 3 — ML Inference (Sentiment, Emotion, Reply)
   ↓ Predictions.parquet
Module 4 — Fine-Tuning (Optional, GPU-aware)
   ↓ Custom small model saved to Drive
Module 5 — Orchestrator (Colab)
   ↓ Full Pipeline Execution & Insights
```

---

# 📦 **Module Breakdown**

## **1️⃣ Module 1 — Scraper**

**Purpose:**
Fetch high-quality video metadata + all comments using the official YouTube API.

**Key Features:**

* Extract video ID from URL
* Fetch metadata (title, views, likes, channel, language)
* Download thousands of comments (top-level + replies)
* Remove creator’s own comments
* Clean & normalize text
* Save data as **Parquet + JSON**

**Core Output:**

```
data/raw/comments_<ID>.parquet  
data/raw/metadata_<ID>.json
```

---

## **2️⃣ Module 2 — Spark Analytics & Topic Modeling**

**Purpose:**
Detect dominant themes in comments and extract structured insights.

**What It Does:**

* Loads Parquet data with PySpark
* Cleans and tokenizes text
* Performs TF-IDF + LDA topic modeling
* Extracts hot topics + keyword relevance
* Generates engagement indicators
* Saves analytics tables for downstream ML

**Core Output:**

```
topics.json  
analytics.parquet
```

---

## **3️⃣ Module 3 — ML Inference (Sentiment, Emotion, Reply Generation)**

**Purpose:**
Apply fast, lightweight models to analyze comments.

**Models Used:**

* **Sentiment (3-class or 5-class)**
* **Emotion classifier (6–8 emotions)**
* **Suggested Reply Generator (small seq-2-seq model)**
* **Toxicity + quality scoring**

**What It Produces:**

* Sentiment label
* Emotion label
* Topic label
* Suggested creator reply
* Comment quality & actionability score

**Core Output:**

```
predictions_<ID>.parquet
```

---

## **4️⃣ Module 4 — Optional Fine-Tuning**

GPU-aware module.
If a GPU is detected → executes fine-tuning.
If not → safely skips, using default models.

**Purpose:**
Adapt sentiment/emotion/reply models to *your channel’s style*.

**Features:**

* Uses the scraped comments as training data
* Fine-tunes a small transformer (DistilBERT / MiniLM)
* Saves trained model versions in Drive

**Output:**

```
data/saved_models/custom_sentiment/
data/saved_models/custom_emotion/
data/saved_models/custom_reply/
```

---

## **5️⃣ Module 5 — Dashboard**

**Purpose:**
Runs the entire system inside Colab.

**What It Does:**

* Detects GPU availability
* Asks user for the YouTube video URL
* Sequentially executes Modules 1 → 4
* Prints interactive summaries
* Shows creator insights, topics, sentiment, best comments, etc.

---

# 🧠 **Core Capabilities**

### ✔ Multi-Label Classification

Sentiment + emotion + toxicity + topic.

### ✔ AI Suggested Replies

Generate meaningful, short responses to comments.

### ✔ Topic Detection (Spark LDA)

Cluster comments into meaningful themes.

### ✔ Creator Insights

* What viewers praise
* What they complain about
* Suggested video ideas
* Best comments to reply to
* Most harmful comments

### ✔ Fully Automated Pipeline

One command processes everything.

---

# 📁 **Folder Structure**

```
YT-Insight/
│
├── data/
│   ├── raw/              # Scraped comments + metadata  
│   ├── proccessed/        # Topic modeling, Spark outputs  
│   ├── predictions/      # Sentiment/emotion results  
│   └── saved_models/     # Fine-tuned models
│
├── YT-insights.ipynb
│
└── README.md
```

---

# ⚙️ **Requirements**

* Python 3.8+
* Google Colab
* Google Drive
* PySpark
* Transformers
* YouTube Data API v3
* Pandas
* Parquet support

---

# 🚀 **How to Run**

1. Open the Colab notebook.
2. Run **Module 5 (Dashboard)**.
3. Enter a YouTube video URL.
4. Let the pipeline execute automatically.
5. Inspect printed insights and generated files.

---

# 🧑‍💻 **Tech Stack**

* **Python**
* **Google Colab**
* **YouTube Data API v3**
* **PySpark**
* **Transformers (HuggingFace)**
* **Pandas / Parquet**
* **Scikit-Learn**
* **Google Drive Persistence**

---

# 🌟 **Why YT-Insight?**

Because creators need more than just likes and view counts.
This tool reveals:

* What people *feel*
* What they *want*
* How the video *performed*
* What themes dominate your audience
* How to respond to comments effectively
* What improvements your content needs

It’s analytics + psychology + AI combined.
