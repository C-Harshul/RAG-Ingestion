RAG-Ingestion — README

Data ingestion + retrieval pipeline using Cloudflare embeddings and ChromaDB.
Base project for the future flagging-mechanism pipeline: when an invoice or bill arrives, the system chunks the document, creates embeddings with Cloudflare, stores them in ChromaDB, and exposes a Retriever to fetch contextual chunks for downstream flagging or classification.
(Repository: https://github.com/C-Harshul/RAG-Ingestion
). 
GitHub

Table of contents

Project overview

Architecture & components

Key features

Installation & prerequisites

Configuration (env)

How it works — ingestion flow

How it works — retrieval flow

Example usage (commands + code snippets)

File / folder overview (high level)

How this fits into the Flagging Mechanism project

Testing / development tips

Contributing & license

1 — Project overview

RAG-Ingestion is a base RAG-style (Retrieval-Augmented Generation) ingestion and retrieval pipeline that prepares document data (invoices, bills, receipts) for downstream analysis. The system:

Breaks documents into semantically-consistent chunks.

Calls Cloudflare embedding model to convert chunks to dense vectors.

Stores vectors + metadata in a ChromaDB vector store.

Exposes a Retriever component which can return contextual chunks given queries (useful for automated flagging, Q&A, or downstream ML models).

This repository contains the core notebook(s) and supporting config for the ingestion + retrieval pipeline. 
GitHub

2 — Architecture & components (high level)

A simple high-level flow:

Document ingestion

Source: PDF / image / text / email attachments.

Preprocessing: OCR if needed, cleaning, extraction of text fields / metadata.

Chunking

Split long documents into smaller chunks (e.g., 500–1,000 tokens / overlapping windows) to preserve context while keeping embedding cost reasonable.

Embedding (Cloudflare)

Send each chunk to the Cloudflare embeddings endpoint (model configured via env) and receive a dense vector.

Index & storage (ChromaDB)

Store vectors + chunk text + metadata (filename, page, invoice_id, date, vendor, etc.) in ChromaDB.

Retriever API or helper

Given a query (or downstream model prompt), run a vector similarity search in Chroma to return k nearest chunks.

Downstream / Flagging

Use the returned contextual chunks to make decisions (rule-based checks, prompting an LLM to identify anomalies, or feed to a classifier).

3 — Key features

Document chunking with overlap to preserve continuity.

Cloudflare embeddings integration for vectorization.

ChromaDB-backed vector store for fast retrieval.

Retriever abstraction to fetch contextual chunks for any downstream task (e.g., flagging invoices with anomalies).

Notebook-based / minimal-code base you can extend into a production pipeline.
