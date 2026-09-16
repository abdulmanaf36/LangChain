# Rerankers in RAG

Retrieval-Augmented Generation (RAG) usually works in two stages:

1. Retrieval: a system fetches a set of candidate documents.
2. Reranking: the retrieved documents are reordered by relevance before being passed to the language model.

The first stage may return relevant but noisy results. Reranking helps improve answer quality by prioritizing the passages that best match the user query.

This folder contains examples of several reranking approaches:

- BM25 reranker
- Cosine similarity reranker
- CrossEncoder reranker
- FlashRank reranker

## Why reranking matters

In a typical RAG pipeline, retrieval is not always enough. A dense or sparse retriever may return documents that are related but not the most useful for the query. Reranking reduces the noise and brings the most relevant passages to the top.

This is especially important when:

- the user query is specific or ambiguous
- many documents are semantically similar
- the final answer depends on ranking the most relevant evidence first

## 1. BM25 reranker

BM25 is a classic lexical ranking algorithm based on term frequency and inverse document frequency. It measures how strongly a query term appears in a document while also accounting for how common or rare that term is across the corpus.

### Key characteristics

- Works well with exact keyword matches
- Fast and interpretable
- Strong for lexical search
- Less effective when the query and document use different wording

### Use case

BM25 is useful when the meaning is mostly conveyed through specific terms, such as technical documents, legal text, or keyword-heavy search systems.

## 2. Cosine similarity reranker

Cosine similarity compares the embedding of the query with the embeddings of candidate documents. It measures the angle between vectors and ranks documents by how closely their vector representations align with the query.

### Key characteristics

- Works with semantic similarity
- Good for matching meaning rather than exact words
- Depends on the quality of the embedding model
- Often used as a simple reranking method after initial retrieval

### Use case

Cosine similarity is useful when the query and document may use different vocabulary but capture the same meaning. It helps handle semantic matching better than purely keyword-based methods.

## 3. CrossEncoder reranker

A CrossEncoder takes the query and a document together as a single input and scores their relevance jointly. Unlike embedding-based approaches, it processes the pair directly and can capture deeper semantic interaction between the two texts.

### Key characteristics

- Usually more accurate than traditional embedding similarity methods
- Captures interaction between query and document text
- More computationally expensive
- Best suited for reranking a smaller set of top candidates

### Use case

CrossEncoders are widely used in modern RAG systems when a small set of high-quality candidates needs to be refined for maximum relevance.

## 4. FlashRank reranker

FlashRank is an efficient reranking library that makes it easier to apply strong reranking models in practical pipelines. It is useful when you want a lightweight and production-friendly reranking layer without building everything from scratch.

### Key characteristics

- Designed for fast reranking
- Good balance between speed and quality
- Useful in real-world retrieval workflows
- Often applied after a retrieval step to improve ranking quality

### Use case

FlashRank is a practical choice for production RAG applications where you want stronger relevance filtering without the overhead of a full custom ranking pipeline.

## Comparison

Each reranker has a different strength:

- BM25 is best for keyword-based matching and is easy to interpret.
- Cosine similarity is best for semantic matching using embeddings.
- CrossEncoder is best for high-quality reranking of a shortlist.
- FlashRank is best for efficient, practical reranking in real systems.

In practice, many RAG systems combine multiple methods:

- retrieve a broad set of candidates with a vector store or BM25
- rerank the top-k with a stronger model such as CrossEncoder or FlashRank
- pass the final ranked documents to the LLM

This gives a better answer quality than relying on raw retrieval scores alone.

## Summary

Reranking is a critical step in modern RAG pipelines. While retrieval decides which documents are considered, reranking decides which ones are actually most useful for the user’s query. The choice of reranker depends on the tradeoff between speed, interpretability, and accuracy.

For simple lexical search, BM25 is powerful. For semantic retrieval, cosine similarity is useful. For stronger relevance matching, CrossEncoder and FlashRank generally provide better ranking quality.

The examples in this folder demonstrate how these methods can be applied in practice and compared side by side.
