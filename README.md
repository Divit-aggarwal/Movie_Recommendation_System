#  Movie Recommender System  
### Content-Based Recommendation Engine (End-to-End)

This project implements a **content-based movie recommender system** from scratch using real world data.  
Every step  from data preprocessing to model design  is **intentional and justified**, with a focus on understanding **why each technique is used instead of alternatives**.

The system recommends movies based on **content similarity**, not user ratings.

---

##  Problem Statement

Given a movie selected by a user, recommend **similar movies** based on:
- Movie overview (plot)
- Genres
- Keywords
- Top cast members
- Director

This approach is especially useful in **coldstart scenarios**, where user interaction data is unavailable.

---

##  Why Content-Based Recommendation?

There are two main recommender approaches:

### Collaborative Filtering (Not Used)
**How it works**
- Uses user item interactions (ratings, likes, watch history)

**Why not used**
- Dataset does not contain user behavior
- Suffers from cold-start problem
- Requires large user–item matrices

---

###  Content-Based Filtering (Used)
**How it works**
- Recommends items based on **item similarity**
- Compares movie metadata instead of users

**Why chosen**
- Works without user data
- Fully explainable
- Ideal for understanding recommender fundamentals
- Matches the dataset structure

---


##  Dataset

**TMDb 5000 Movie Dataset**

Files used:
- `tmdb_5000_movies.csv`
- `tmdb_5000_credits.csv`

**Why this dataset**
- Real world, messy data
- Rich metadata (genres, cast, crew, keywords)
- Common benchmark for recommender systems

---

##  Data Preprocessing

> Most of the intelligence in this system comes from careful preprocessing.

---

###  Dataset Merging

The two datasets are merged on `movie_id` to bring all relevant movie information into a single table.

**Why**
- Metadata and cast/crew data are split across files
- Recommender requires a unified representation

---

###  Feature Selection

**Columns Used**
- `movie_id`
- `title`
- `overview`
- `genres`
- `keywords`
- `cast`
- `crew`

**Columns Dropped**
- `budget`, `revenue`, `homepage`, `runtime`, etc.

**Why dropped**
- No semantic value for content similarity
- Add noise and unnecessary complexity

---

###  Handling Missing and Duplicate Data

- Rows with missing values are removed
- Duplicate movies are dropped

**Why**
- Vectorization cannot handle null values
- Duplicates distort similarity calculations

---

###  Parsing Nested JSON-like Data

Columns like `genres`, `keywords`, `cast`, and `crew` are stored as **stringified Python objects**.

Example:
"[{'id': 28, 'name': 'Action'}]"

---

### 5️⃣ Feature Extraction Strategy

####  Cast
- Only **top 3 cast members** are extracted

**Why**
- Lead actors define a movie’s identity
- Full cast adds noise and dimensionality

---

####  Crew
- Only the **director** is extracted

**Why**
- Director strongly influences storytelling and style
- Other roles add limited semantic value

---

### 6️⃣ Text Normalization

#### Removing Spaces
Science Fiction" → "ScienceFiction"


#### Lowercasing
"Action" == "action"


##  Feature Engineering: `tags` Column

All meaningful textual information is combined into a single column:

tags = overview + genres + keywords + cast + crew


**Why**
- Treats each movie as a single document
- Simplifies vectorization
- Enables unified similarity computation

---

##  Vectorization

### Technique Used: **CountVectorizer**

**What it does**
- Converts text into numerical vectors
- Uses word frequency (Bag of Words)

---

### Why CountVectorizer over TF-IDF?

| CountVectorizer          |    TF-IDF          |
|--------------------------|--------------------|
| Simple & interpretable   | More complex       |
| Faster                   | Slightly slower    |
| Well for curated tags    | for long documents |


##  Similarity Metric

### Used: **Cosine Similarity**

**Why**
- Measures orientation, not magnitude
- Ideal for sparse, high-dimensional text vectors
- Industry standard for NLP similarity tasks

**Why not Euclidean Distance**
- Sensitive to vector length
- Performs poorly with sparse data

---

##  Recommendation Logic

1. Select a movie
2. Retrieve its vector representation
3. Compute cosine similarity with all movies
4. Sort by similarity score
5. Recommend top similar movies

---

## ----------- More Coming soon ----------

