---
title: "Searching Techniques"
date: 2025-07-22
tags: ["System Design", "Database Engineering", "Search", "PostgreSQL", "Elasticsearch", "Vector Databases"]
description: "Explore lexical and semantic search techniques, their key differences, and practical implementations using PostgreSQL, Elasticsearch, and vector databases."
draft: true
---

## Lexical Search

Lexical search—also called **keyword search**—relies on matching the **surface form** of a query: exact words, character sequences, or partial matches. It doesn’t "understand" language or meaning.

> **Lexical search does not understand context or intent—it matches exact or near-exact words.**

Matches query terms **exactly**, optionally case-insensitively.
```sql
SELECT * FROM users WHERE name = 'Alice';
````

## Full Text Search

Suppose you want to search a collection of blog posts or reviews using a natural language query like:

> "space exploration in the 1960s"

A naive query like:

```sql
SELECT * FROM articles WHERE content LIKE '%space%';
```

…won’t handle relevance, synonyms, or ranking. Full-text search solves this.

### Key Concepts

* **Tokenization**: Split sentences into individual words
* **Stemming**: Normalize word forms (`running` → `run`)
* **Stop-word removal**: Remove common words (`the`, `a`, etc.)
* **Inverted Index**: Map tokens → documents

### PostgreSQL Full-Text Search Example

```sql
CREATE TABLE articles (
  id SERIAL PRIMARY KEY,
  title TEXT,
  content TEXT,
  tsv tsvector
);

INSERT INTO articles (title, content, tsv) VALUES (
  'Mars Mission',
  'NASA launched a new mission to Mars with advanced AI.',
  to_tsvector('english', 'NASA launched a new mission to Mars with advanced AI.')
);

CREATE INDEX idx_fts ON articles USING GIN(tsv);

-- Search: documents containing both 'mars' and 'ai'
SELECT id, title
FROM articles
WHERE tsv @@ to_tsquery('english', 'mars & ai');

-- Ranked results
SELECT id, title, ts_rank(tsv, to_tsquery('mars & ai')) AS rank
FROM articles
WHERE tsv @@ to_tsquery('mars & ai')
ORDER BY rank DESC;
```
### Full-Text Search with Elasticsearch

```bash
# Create Index
curl -X PUT localhost:9200/articles -H "Content-Type: application/json" -d '
{
  "mappings": {
    "properties": {
      "title":    { "type": "text", "analyzer": "english" },
      "content":  { "type": "text", "analyzer": "english" }
    }
  }
}'

# Index Document
curl -X POST localhost:9200/articles/_doc/1 -H "Content-Type: application/json" -d '
{
  "title": "Mars Mission",
  "content": "NASA launched a new mission to Mars with advanced AI."
}'

# Search
curl -X GET localhost:9200/articles/_search -H "Content-Type: application/json" -d '
{
  "query": {
    "match": {
      "content": "mars ai"
    }
  }
}'
```

## Semantic Search

Semantic search focuses on **meaning and intent**, not exact terms. It uses **embeddings**, **language models**, and **vector similarity** to match queries and content based on **contextual understanding**.

> "best laptops for graphic design students" → not just keyword match, but understanding needs: powerful GPU, color-accurate display, etc.

### How Semantic Search Works

1. **Query Understanding**: NLP extracts **entities**, **intent**, and **context**.
2. **Embedding & Vectorization**: Converts queries and documents into dense vectors using models like BERT, OpenAI, or Gemini.
3. **Similarity Matching**: Finds closest vectors using cosine similarity or other metrics.
4. **Ranking**: Uses semantic distance, relevance, or even knowledge graphs for final ranking.

### Technologies

* **Embeddings**: OpenAI, Gemini, Cohere, Sentence Transformers
* **Vector DBs**: FAISS, pgvector, Pinecone, Weaviate, Redis
* **NLP**: Named Entity Recognition, Sentiment Analysis, etc.
* **Knowledge Graphs**: Google Knowledge Graph, Wikidata


## Full-Text vs Semantic Search


| Feature     | Full-Text Search            | Semantic Search                           |
| ----------- | --------------------------- | ----------------------------------------- |
| Match Type  | Token/word-based            | Meaning/intent-based                      |
| Tools       | PostgreSQL, Elasticsearch   | FAISS, pgvector, Pinecone, Redis          |
| Strengths   | Fast, precise keyword match | Captures deeper meaning, handles synonyms |
| Limitations | No understanding of context | Heavier compute, model/infra needed       |

> Query: **"astronaut survives on red planet"**

| System               | Match                               | Why?                                                 |
| -------------------- | ----------------------------------- | ---------------------------------------------------- |
| **Full-Text Search** | `"NASA launched a mission to Mars"` | Only if “Mars” appears — doesn't match "red planet"  |
| **Semantic Search**  | `"NASA launched a mission to Mars"` | Understands "red planet" means "Mars" — deeper match 

> Choose **full-text search** for keyword precision.
> Choose **semantic search** for contextual relevance.


## 🎥 Further Learning

* [How Full-Text Search Works (YouTube)](https://youtu.be/b7tCjZSvOno?list=TLGGNQOC-KmuKk4yMTA3MjAyNQ)
* [PostgreSQL Full Text Search Docs](https://www.postgresql.org/docs/current/textsearch-intro.html)
* [Weaviate: Distance Metrics in Vector Search](https://weaviate.io/blog/distance-metrics-in-vector-search)

