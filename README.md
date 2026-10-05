# Movie Recommendation System

A content-based movie recommender built with the TMDB 5000 Movie Dataset. The project turns each movie's metadata into a collection of descriptive tags, compares movies using cosine similarity, and returns the five most similar titles.

## How it works

The notebook follows this pipeline:

1. Load the movies and credits datasets.
2. Merge both datasets using the movie title.
3. Keep useful features: overview, genres, keywords, cast, and director.
4. Clean the data and combine the features into a single `tags` field.
5. Apply stemming with NLTK's `PorterStemmer`.
6. Convert the tags into Bag-of-Words vectors using `CountVectorizer`.
7. Calculate pairwise cosine similarity.
8. Recommend the five closest movies for a given title.

For example, the notebook can generate recommendations with:

```python
recommend("Titanic")
```

## Project structure

```text
.
|-- code.ipynb
|-- data/
|   |-- tmdb_5000_movies.csv
|   `-- tmdb_5000_credits.csv
|-- notes.md
`-- README.md
```

- `code.ipynb` contains the complete preprocessing, feature-engineering, model-building, and recommendation workflow.
- `data/` contains the two CSV files used by the notebook.
- `notes.md` contains early project notes and background on recommendation-system types.

## Dataset

This project uses the TMDB 5000 Movie Dataset:

- `tmdb_5000_movies.csv` - movie metadata such as genres, keywords, overview, popularity, and release information.
- `tmdb_5000_credits.csv` - cast and crew information for each movie.

The two files have a combined size of approximately **43.6 MB** (`38.2 MB` for credits and `5.4 MB` for movies). To keep the repository lightweight, the dataset can be downloaded separately when setting up the project.

### Download from Kaggle

#### Option 1: Download in a browser

1. Open the [TMDB 5000 Movie Dataset on Kaggle](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata).
2. Sign in to Kaggle and select **Download**.
3. Extract the downloaded archive.
4. Create a `data` directory in the project root if it does not already exist.
5. Copy both CSV files into `data/`.

The resulting structure must be:

```text
data/
|-- tmdb_5000_movies.csv
`-- tmdb_5000_credits.csv
```

#### Option 2: Download with the Kaggle CLI

Install and authenticate the Kaggle CLI, then run this command from the project root:

```bash
kaggle datasets download -d tmdb/tmdb-movie-metadata -p data --unzip
```

Confirm that both CSV files are directly inside `data/`, not in an additional nested directory. The notebook expects these relative paths:

```text
data/tmdb_5000_movies.csv
data/tmdb_5000_credits.csv
```

## Requirements

- Python 3.9 or newer
- Jupyter Notebook or JupyterLab
- NumPy
- pandas
- NLTK
- scikit-learn

Install the required packages with:

```bash
python -m pip install numpy pandas nltk scikit-learn jupyter
```

The project only uses NLTK's `PorterStemmer`, so no additional NLTK corpus download is needed.

## Run the project

1. Clone or download this repository.
2. Open a terminal in the project directory.
3. Download the dataset using one of the methods above if the CSV files are not present.
4. Install the dependencies shown above.
5. Start Jupyter:

   ```bash
   jupyter notebook
   ```

6. Open `code.ipynb` and run the cells from top to bottom.
7. Call `recommend()` with a movie title that exists in the dataset.

The notebook uses relative paths, so keep the two CSV files inside the `data/` directory when running it.

## Recommendation approach

This is a **content-based** recommendation system. It recommends movies with metadata similar to the selected movie; it does not use user ratings or viewing history.

The model uses:

- movie overview
- genres
- keywords
- the top three cast members
- director names

After preprocessing, these features are combined and vectorized into a maximum of 5,000 Bag-of-Words features. Cosine similarity is then used to rank related movies.

## Current limitations

- Movie titles must match the dataset exactly.
- The model does not yet handle unknown or misspelled titles.
- Recommendations are based on metadata rather than user preferences.
- The full similarity matrix is held in memory.
- The project currently runs in a notebook and does not yet include an API or user interface.

## Possible improvements

- Add case-insensitive and fuzzy title search.
- Return similarity scores with each recommendation.
- Build a Streamlit or web interface.
- Expose the recommender through an API.
- Persist processed data and similarity results for faster startup.
- Add movie posters and additional TMDB metadata.

## Acknowledgements

The movie and credits data originates from The Movie Database (TMDB) and is used here for learning and experimentation.
