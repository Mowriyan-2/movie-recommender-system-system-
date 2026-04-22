# movie-recommender-system-system-
# 🎬 Movie Recommender System

A **content-based movie recommendation system** built with Python and Machine Learning that suggests the top 5 similar movies for any given movie title — based on genres, keywords, cast, crew, and plot overview.


## 📌 What is a Recommender System?

A recommender system is a type of information filtering system that predicts what a user might like based on various factors. There are three main types:

| Type | Description | Example |
|---|---|---|
| **Content-Based** | Recommends based on item features | "You liked Inception, try Interstellar" |
| **Collaborative Filtering** | Recommends based on user behavior | "Users like you also watched..." |
| **Hybrid** | Combines both approaches | Netflix, Spotify |

This project implements a **Content-Based Filtering** approach.

---

## 🧠 How It Works

The system follows this pipeline:

```
Raw Data → Preprocessing → Feature Extraction → Vectorization → Similarity → Recommendation
```

### Step-by-step breakdown:

**1. Data Loading & Merging**
- Loads `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv`
- Merges them on the `title` column

**2. Feature Selection**
- Keeps only the most relevant columns:
  `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`, `crew`

**3. Data Preprocessing**
- Parses JSON-like strings in `genres`, `keywords`, `cast`, `crew` using `ast.literal_eval`
- Extracts top 3 cast members per movie
- Extracts only the **Director** from the crew
- Removes spaces from multi-word names (e.g. `Sam Mendes` → `SamMendes`) to avoid ambiguity in vectorization

**4. Tag Creation**
- Combines `overview + genres + keywords + cast + crew` into a single `tags` string per movie

**5. Vectorization**
- Uses **CountVectorizer** with `max_features=5000` and English stop words removed
- Converts each movie's tags into a 5000-dimensional vector

**6. Cosine Similarity**
- Computes cosine similarity between all movie vectors
- Higher similarity score → more similar movies

**7. Recommendation**
- Given a movie title, fetches its index
- Sorts all movies by similarity score (descending)
- Returns the top 5 most similar movies

---

## 🚀 Sample Output

```python
recommend('The Dark Knight')
# → The Dark Knight Rises
# → Batman Begins
# → Batman
# → Kick-Ass
# → Shazam

recommend('Inception')
# → The Dark Knight
# → Interstellar
# → The Prestige
# → Memento
# → Shutter Island
```

### Movies tested across genres:

| Movie | Genre |
|---|---|
| Gandhi | Biography / Drama |
| The Dark Knight | Action / Crime |
| Interstellar | Sci-Fi |
| The Lion King | Animation / Family |
| Titanic | Romance / Drama |
| The Shining | Horror |
| Home Alone | Comedy |
| The Avengers | Superhero / Action |
| Inception | Thriller / Sci-Fi |

---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 | Core language |
| Pandas | Data loading & manipulation |
| NumPy | Numerical operations |
| Scikit-learn | CountVectorizer & cosine similarity |
| AST | Parsing JSON-like strings |
| KaggleHub | Dataset download |
| Jupyter Notebook | Development environment |

---

## 📂 Dataset

**TMDB 5000 Movie Dataset** from Kaggle — contains metadata for 5000 movies scraped from The Movie Database (TMDb).

Files used:
- `tmdb_5000_movies.csv` — movie details (genres, keywords, overview, etc.)
- `tmdb_5000_credits.csv` — cast and crew information

Download via KaggleHub:
```python
import kagglehub
import os

path = kagglehub.dataset_download("tmdb/tmdb-movie-metadata")
movies = pd.read_csv(os.path.join(path, 'tmdb_5000_movies.csv'))
credits = pd.read_csv(os.path.join(path, 'tmdb_5000_credits.csv'))
```

Or download manually: [Kaggle Dataset Link](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

---

## 📁 Project Structure

```
movie-recommender-system/
│
├── movierecommender.ipynb   # Full ML pipeline — preprocessing, vectorization, recommendation
└── README.md                # Project documentation
```

---

## ⚙️ Setup & Run

**1. Clone the repository**
```bash
git clone https://github.com/Mowriyan-2/movie-recommender-system-system-.git
cd movie-recommender-system-system-
```

**2. Install dependencies**
```bash
pip install pandas numpy scikit-learn kagglehub jupyter
```

**3. Launch the notebook**
```bash
jupyter notebook movierecommender.ipynb
```

**4. Run all cells** — the last cells let you call `recommend('Movie Title')` with any movie name from the dataset.

---

## 💡 Key Concepts Used

- **Bag of Words (BoW)** — CountVectorizer converts text tags into word frequency vectors
- **Cosine Similarity** — Measures the angle between two vectors; closer to 1 = more similar
- **Text Preprocessing** — Removing stop words, collapsing spaces to avoid tag ambiguity
- **Feature Engineering** — Combining multiple metadata fields into a unified tag

## 🙋‍♂️ Author

**Mowriyan** — [GitHub](https://github.com/Mowriyan-2)
