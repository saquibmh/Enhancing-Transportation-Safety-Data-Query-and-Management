# Arizona Crash Facts Analysis with RAG

This repository contains code and resources for analyzing the "Arizona Crash Facts 2023" PDF document using Retrieval Augmented Generation (RAG). It demonstrates three different methods for processing the PDF and querying the data using LangChain and Hugging Face embeddings with OpenAI's GPT-4o-mini.

## Project Overview

The project aims to extract information from the "Arizona Crash Facts 2023" PDF, which contains various tables and text related to traffic accidents in Arizona. We explore different strategies for processing the document and retrieving relevant information in response to user queries.

## Methods

1.  **Method 1: Direct PDF Text Extraction:**
    * This method extracts all the text from the PDF using `pypdf` and indexes it directly.
    * LangChain's `TextLoader` and `RecursiveCharacterTextSplitter` are used to load and chunk the text.
    * Hugging Face embeddings and `Annoy` are used for vector storage and retrieval.
    * LangChain's RAG prompt and OpenAI's `ChatOpenAI` are used to answer queries.
2.  **Method 2: Table Separation with Document Intelligence:**
    * This method uses a pre-processed Markdown file (`cleaned_output.md`) where tables are separated from the text.
    * The Markdown is parsed to extract tables and text separately.
    * Tables and text are indexed using Hugging Face embeddings and `Annoy`.
    * LangChain's RAG pipeline is used for querying.
3.  **Method 3: Table Summarization with LLM:**
    * This method uses a pre-generated summary of the tables (`summarized_tables_prompt2.md`).
    * The tables and their summaries are combined.
    * The combined text is indexed using Hugging Face embeddings and Annoy.
    * LangChain's RAG pipeline is used for querying.

## Requirements

* Python 3.7+
* pip
* OpenAI API Key
* Hugging Face Transformers

Install the required Python packages:

```bash
pip install langchain-huggingface langchain_community annoy pypdf
