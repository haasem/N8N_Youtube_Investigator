# 📊 YouTube Investigator — N8N Flow Documentation

## 🧠 Overview

**YouTube Investigator** is an automated content intelligence pipeline built in [n8n](https://n8n.io) for analyzing and extracting high-performing patterns from YouTube channels. It helps content creators and strategists extract **power words**, **thumbnail hooks**, and **audience insights** across niche-specific YouTube videos, and compiles them into actionable ideation sheets.

<img width="954" alt="image" src="https://github.com/user-attachments/assets/03239da1-d384-47bd-99c1-26c5d5dd6f35" />

## 🗂️ Workflow Phases

### Phase 1: 🎯 _Niche Outliers (Manual Input)_

- **Trigger**: Form input of 3 high-quality YouTube channel URLs.
- **Nodes**:
  - `FormTrigger`: Accepts 3 YouTube URLs from the user.
  - `Set`: Saves them as an array `channels`.
  - `SplitOut`: Splits each channel URL for parallel processing.

### Phase 2: 🌐 _Data Extraction (Channel-Based)_

- **Loop**: Iterates over each channel.
- **Nodes**:
  - `HTTP Request`: Scrapes latest 30 videos from each channel (via Apify).
  - `Sort`: Orders by view count.
  - `Filter`: Keeps videos newer than 6 months.
  - `Limit`: Restricts to top 10.
  - `Set Fields`: Extracts fields like title, view count, thumbnail, etc.

### Phase 3: 💬 _Title & Thumbnail Analysis_

- **Nodes**:
  - `LangChain Agent`: Analyzes each title and returns a list of 1–3 “power words”.
  - `LangChain (Image Model)`: Analyzes each thumbnail for curiosity, promise, visual impact.

### Phase 4: 📋 _Data Aggregation & Export_

- **Node**: `Merge`
  - Combines title, power words, thumbnail, views, likes.
- **Output**: Google Sheet (`Niche Outliers Data` tab)
  - Columns include: `Channel`, `Title`, `URL`, `Views`, `Power Words`, `Thumbnail Analysis`, etc.


## 📅 Automated Phases

### 🔁 Weekly: Broad Niche Insights

- **Trigger**: Runs every Sunday at 5am.
- **Niche Set**: e.g. `"Business Process Automation"`
- **Flow**:
  - Scrapes videos in the niche (top 5 weekly).
  - Extracts power words and analyzes thumbnails.
  - Stores results in Google Sheet tab: `Broad Niche Weekly`.

### 🔁 Every 3 Days: Niche Daily Insights

- **Trigger**: Every 3 days at 6am.
- **Niche Set**: e.g. `"Agentic AI"`
- **Flow**:
  - Scrapes top 5 videos in the niche (by views).
  - Stores daily snapshot in `Niche Daily` tab with date.


## 💬 Audience Comment Analysis

- **Flow**:
  - Scrapes comments from each video.
  - Uses LLM to summarize:
    - What audiences love 👍
    - What they dislike 👎
    - What they request next ❓
- **Output**: Google Sheet `Comment Analysis`


## 💡 Ideation Agent

- **Input**: Aggregated titles, power words, thumbnail hooks, comment summaries.
- **LLM Agent**:
  - Generates **3 new video title + thumbnail concepts**.
  - Tailored to **SMB audiences**, with variety in tone/format.
- **Output**:
  - Stored in `Ideation` tab.
  - Email sent via Gmail node when ideation is complete.


## 📁 Sheet References

| Sheet Name              | Purpose                                      |
|-------------------------|----------------------------------------------|
| `Niche Outliers`        | Manual submission results                    |
| `Broad Niche Weekly`    | Weekly scraped niche-wide analysis           |
| `Niche Daily`           | Auto-scraped insights every 3 days           |
| `Comment Analysis`      | Summary of YouTube audience comments         |
| `Ideation`              | AI-generated titles + thumbnails             |


## 🔐 Credentials Used

- **Google Sheets API** (OAuth2)
- **OpenAI API** (via LangChain nodes)
- **Apify API** (YouTube scraping)
- **Gmail API** (Notification delivery)


## ⚙️ Requirements

- `n8n` hosted (self-hosted or Cloud)
- Environment variables:
  - `APIFY_TOKEN`
  - OpenAI key with access to GPT-4 or GPT-4o
  - Google/Gmail OAuth2 credentials


## Example Use Cases
- Content marketing teams validating YouTube trends.
- Agencies optimizing thumbnails and titles.
- Strategists building performance-based ideation pipelines.
