# Semantic Search over Yelp Restaurant Reviews

Course project for **Text Data Mining** (Master's programme, semester 3).

## Goal

Build a search engine that lets a user describe what they are looking for in free text (e.g. _"quiet cafe to work from"_, _"pizza with fruit"_, _"good vegan options"_) and returns restaurants whose reviews best match the query, even when the review does not contain the same words. We compare classical keyword search (BM25) with semantic search based on sentence embeddings, and wrap the best approach in a simple interactive UI.

## Data source

We use the **[Yelp Open Dataset](https://www.yelp.com/dataset)**, a public dataset released by Yelp for academic and non-commercial use. It contains business metadata and user reviews, mainly from US and Canadian cities. The text is in English.

The archive (`yelp_dataset.tar`) contains JSON Lines files (one JSON object per line). We use two of them:

**`yelp_academic_dataset_business.json`**: one row per business

| Field                              | Description                                                 |
| ---------------------------------- | ----------------------------------------------------------- |
| `business_id`                      | Unique business key                                         |
| `name`, `address`, `city`, `state` | Basic information                                           |
| `latitude`, `longitude`            | Location                                                    |
| `stars`, `review_count`            | Average rating and number of reviews                        |
| `categories`                       | Comma-separated tags, e.g. `"Restaurants, Pizza, Italian"`  |
| `attributes`, `hours`, `is_open`   | Additional metadata (price range, amenities, opening hours) |

**`yelp_academic_dataset_review.json`** (about 5 GB): one row per review

| Field                             | Description                                  |
| --------------------------------- | -------------------------------------------- |
| `review_id`, `user_id`            | Identifiers                                  |
| `business_id`                     | Foreign key to the business file             |
| `stars`                           | Review rating (1-5)                          |
| `text`                            | Review text (the main input for text mining) |
| `date`, `useful`, `funny`, `cool` | Date and vote counts                         |

The remaining files (like `user`, `tip`, `checkin`, `photo`) are not used.

### Subset used in the project

- businesses with the `Restaurants` category,
- a single city (Philadelphia) to keep the corpus consistent and the embedding step fast,
- at least 30 reviews per business,
- at most 100 randomly sampled reviews per business.

### Data licence

The dataset is subject to the Yelp Dataset licence (non-commercial / academic use only, redistribution not allowed). **The data is not stored in this repository.** To reproduce the project, request the dataset from the Yelp website and place `yelp_dataset.tar` in the shared project folder (see _Setup_).

## Project plan

1. **Data preparation**: load business metadata, filter restaurants, read the review file in chunks, sample reviews per business, save the result as Parquet.
2. **Exploratory data analysis**: distribution of ratings and review length, reviews per business, categories, duplicates, very short or empty texts, most frequent terms.
3. **Preprocessing**: light text cleaning and splitting long reviews into shorter chunks (a single review usually covers several topics).
4. **Baseline**: keyword search with TF-IDF / BM25.
5. **Semantic search**: sentence embeddings (`sentence-transformers`), vector index (FAISS or Chroma), cosine similarity. Compare several embedding models.
6. **Aggregation to restaurant level**: turn chunk-level scores into a restaurant ranking (e.g. mean of top-k chunks vs. maximum), optionally combined with star ratings.
7. **Hybrid search and filters**: combine BM25 with embeddings; filter by rating, category and price.
8. **Evaluation**: a hand-labelled set of 20-30 queries of different types (dish, atmosphere, diet, service, negation), metrics such as Precision@k and NDCG, error analysis (negation, irony, rare ingredients).
9. **User interface**: interactive app with a query box, filters, ranked restaurants and the most relevant review excerpts.

## Repository structure

```
tba
```

## Setup

The project runs on **Google Colab**. Large files (raw data, Parquet files, embeddings) are kept on a shared Google Drive folder (`yelp_project/`), not in Git.

tba

## Authors

- Iza Siemieniuk
- Natalia Kubańska
