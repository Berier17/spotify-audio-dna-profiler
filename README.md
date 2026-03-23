# 🎵 Music Analytics — Do Lyrics Even Matter?

An end-to-end data analytics project exploring whether a song's audio characteristics drive its popularity more than its lyrical content.

## 📌 The Theory
Lyrics have nothing to do with whether a song is energetic, danceable, catchy, or gives you the right feeling. 

What makes a song popular is its audio DNA — energy, danceability, tempo, valence — not what the artist is saying. This project puts that theory to the test using real data from 89,740 Spotify tracks.

---

## 📸 Dashboard Preview
*<img width="1545" height="858" alt="Overview" src="https://github.com/user-attachments/assets/9cf67a8a-2ab1-406b-92b5-a531d964da38" />*

---

## 🛠️ Tools & Stack

| Stage | Tool |
| :--- | :--- |
| **Data Collection** | Python (Requests, Pandas), Last.fm API |
| **Data Source** | Kaggle (Spotify Dataset), Last.fm |
| **Data Cleaning** | Python (Pandas) |
| **Database** | PostgreSQL (pgAdmin) |
| **Visualization** | Power BI Desktop |

---

## 📁 Project Structure
```text
music-analytics/
│
├── data/
│   ├── dataset.csv                  # Original Kaggle Spotify dataset
│   ├── songs_with_tags.csv          # Dataset enriched with Last.fm genre tags
│   └── tables/
│       ├── fact_songs.csv
│       ├── dim_artist.csv
│       ├── dim_genre.csv
│       ├── dim_audio_features.csv
│       └── bridge_song_artist.csv
│
├── notebooks/
│   └── Spotify.ipynb                # Full data pipeline notebook
│
├── music_analytics_dashboard.pbix   # Power BI report
└── README.md
```
---
## 🔄 Pipeline Overview

### 1. Data Collection
* Downloaded a 114,000-song Spotify dataset from Kaggle containing audio features, popularity scores, and metadata.
* Built a Python pipeline to enrich each song with genre tags from the Last.fm API, using track-level tags with an artist-level fallback and an artist cache to avoid redundant API calls.
* Handled rate limiting, resume logic with checkpoints every 500 rows, and parallel processing.

### 2. Data Cleaning & Transformation
* Removed duplicates, null values, and junk columns.
* Converted duration from milliseconds to seconds.
* Split semicolon-separated artist collaborations (e.g. `Drake;Future`) into individual artist rows.
* Remapped foreign keys after artist splitting to maintain referential integrity.
* Split data into a star schema with 4 tables.

### 3. Database — PostgreSQL
Built a star schema in PostgreSQL with the following tables:

```text
                    ┌─────────────┐
                    │  dim_artist │
                    │  artist_id  │
                    │ artist_name │
                    └──────┬──────┘
                           │
┌─────────────┐    ┌──────▼──────┐    ┌──────────────────┐
│  dim_genre  │    │  fact_songs │    │dim_audio_features│
│  genre_id   ├────│  track_id   ├────│  track_id        │
│ track_genre │    │  artist_id  │    │  danceability    │
│  genre_tag  │    │  genre_id   │    │  energy          │
└─────────────┘    │  track_name │    │  valence         │
                   │  album_name │    │  tempo           │
                   │  popularity │    │  loudness        │
                   │ duration_sec│    │  speechiness     │
                   │  explicit   │    │  acousticness    │
                   └─────────────┘    │ instrumentalness │
                                      │  liveness        │
                                      │  key, mode       │
                                      │  time_signature  │
                                      └──────────────────┘
```
### 4. Power BI Dashboard
Connected Power BI directly to PostgreSQL. Built a 3-page report with 27 DAX measures organized into display folders inside a dedicated `_Measures` table.

---

## 📊 DAX Measures

**General**
* `Total Songs` — total track count
* `Avg Popularity` — average Spotify popularity (0–100)
* `Total Artists` — unique individual artists
* `Total Genres` — unique genre count

**Audio Features**
* `Avg Energy`, `Avg Danceability`, `Avg Valence`, `Avg Tempo`, `Avg Loudness`, `Avg Acousticness`, `Avg Speechiness`, `Avg Instrumentalness`, `Avg Liveness`

**Theory Analysis**
* `High Energy Songs` / `High Danceability Songs` / `High Energy & Danceable`
* `Avg Popularity High Energy` vs `Avg Popularity Low Energy`
* `Avg Popularity High Danceability` vs `Avg Popularity Low Danceability`
* `Energy vs Popularity Gap` — positive = high energy songs are more popular
* `Danceability vs Popularity Gap` — positive = danceable songs are more popular
* `Explicit vs Non-Explicit Popularity` — tests if lyrical content affects popularity
* `% High Energy & Popular`
* `Total Sad Bangers` — songs that are danceable (≥0.70) but emotionally negative (valence ≤0.40)
* `Avg Popularity Sad Bangers` — avg popularity of sad bangers
* `% of Popular Songs that are Sad Bangers` — among songs with popularity >75
