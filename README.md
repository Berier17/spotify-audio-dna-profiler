# 🎵 Music Analytics — Do Lyrics Even Matter?

An end-to-end data analytics project exploring whether a song's audio characteristics drive its popularity more than its lyrical content.

## 📌 The Theory
Lyrics have nothing to do with whether a song is energetic, danceable, catchy, or gives you the right feeling. 

What makes a song popular is its audio DNA — energy, danceability, tempo, valence — not what the artist is saying. This project puts that theory to the test using real data from 89,740 Spotify tracks.

---

## 📸 Dashboard Preview
*[Insert a high-resolution screenshot of your main Power BI dashboard page here]*

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
