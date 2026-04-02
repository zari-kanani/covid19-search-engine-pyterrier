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

### Retrieval Models

| Model | Description |
|---|---|
| **TF** | Ranks by raw term frequency |
| **TF-IDF** | Weighs frequency against term rarity across the corpus |
| **BM25** | Extends TF-IDF with document length normalization |


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

