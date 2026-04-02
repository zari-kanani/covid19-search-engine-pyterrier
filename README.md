# COVID-19 Literature Search Engine

A full-text search engine over a corpus of COVID-19 scientific literature, built with Python and the [PyTerrier](https://github.com/terrier-org/pyterrier) IR framework.

## Overview

This project implements a complete Information Retrieval pipeline — from raw data to ranked search results — using classical IR models on a real-world scientific dataset.

The pipeline covers three stages:

1. **Indexing** — Constructing an inverted index over the document corpus using PyTerrier's `DFIndexer`.
2. **Retrieval** — Querying the index using three classical ranking models: TF, TF-IDF, and BM25.
3. **Analysis** — Comparing model outputs and evaluating retrieval quality.

## Dataset

The dataset (`covid_df.csv`) contains COVID-19 scientific articles with the following fields:

| Column | Description |
|---|---|
| `doc_id` | Unique document identifier |
| `Title` | Article title |
| `Abstract` | Article abstract |
| `Year` | Publication year |

## Project Structure

```
covid19-search-engine/
├── covid19_search_engine_pyterrier.ipynb   # Main notebook
├── covid_df.csv                            # Dataset
├── requirements.txt                        # Python dependencies
└── README.md
```

## Key Concepts Covered

### Index Structures
- **Meta index** — stores document-level metadata (docno, title, abstract)
- **Lexicon** — stores all indexed terms and their corpus-wide statistics
- **Inverted index** — maps each term to the list of documents containing it

### Retrieval Models

| Model | Description |
|---|---|
| **TF** | Ranks by raw term frequency |
| **TF-IDF** | Weighs frequency against term rarity across the corpus |
| **BM25** | Extends TF-IDF with document length normalization |

### Text Preprocessing
Queries are preprocessed through the same pipeline used during indexing:
- Tokenization
- Stemming (Porter Stemmer)
- Stopword removal

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/covid19-search-engine.git
cd covid19-search-engine
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

> **Note:** PyTerrier requires Java. Make sure you have **Java 11+** installed on your system.  
> Check with: `java -version`

### 3. Run the notebook

Open the notebook in Jupyter or Google Colab:

```bash
jupyter notebook covid19_search_engine_pyterrier.ipynb
```

Or open directly in [Google Colab](https://colab.research.google.com/) and upload `covid_df.csv` when prompted.

## Example Usage

```python
import pyterrier as pt

# Initialize PyTerrier
if not pt.started():
    pt.init()

# Build retrieval models
tf_idf = pt.BatchRetrieve(index, wmodel="TF_IDF", metadata=["Title", "Abstract"])
bm25   = pt.BatchRetrieve(index, wmodel="BM25",   metadata=["Title", "Abstract"])

# Run a query
results = bm25.search("clinical trials in covid")
print(results.head(5))
```

## Results

Comparing the three models on the query `"clinical trials in covid"`:

- **TF** — ranks by raw term counts; tends to favor longer documents.
- **TF-IDF** — reduces weight of common terms; more discriminative results.
- **BM25** — balances term frequency with document length normalization; most balanced and widely used in practice.

## Dependencies

See [`requirements.txt`](requirements.txt) for the full list. Main libraries:

- `python-terrier` — indexing and retrieval
- `pandas`, `numpy` — data manipulation
- `nltk` — text preprocessing
- `matplotlib` — visualization

## License

This project is for educational purposes. The dataset is derived from publicly available COVID-19 research literature.
