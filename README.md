# NavyaNAYAK-CLAUSE_AI

# Project README: Clause AI - Contract Analysis



## 1. Data Loading and Initial Exploration

*   **Source Data**: The project utilizes the CUAD v1 dataset, consisting of a JSON file (`CUAD_v1.json`) with annotated clauses and a directory of raw contract text files (`full_contract_txt`). A `master_clauses.csv` file provides additional clause metadata.
*   **Loading**: Both the JSON data and individual `.txt` contract files were loaded into Pandas DataFrames.
*   **Initial Analysis**: Performed exploratory data analysis (EDA) on the raw text files to understand contract lengths (word and character counts). Visualizations such as histograms and boxplots were generated to show the distribution of contract sizes. WordClouds and bar charts were used to identify the most frequent terms in the corpus.
*   **Key Insight**: Identified that contracts vary significantly in length, necessitating a chunking strategy for processing.

## 2. Text Cleaning and Normalization

*   **Objective**: To prepare raw contract text for further NLP processing by removing noise and standardizing format.
*   **Preprocessing Steps**: Applied a series of cleaning operations:
    *   Lowercase conversion.
    *   Removal of special characters and punctuation.
    *   Normalization of whitespace.
    *   Removal of common English stopwords.
    *   Fixing hyphenation issues (`-
`).
    *   Removing short lines (potential headers/footers).
    *   Removing non-ASCII characters.
*   **Output**: Cleaned text files were saved to a `data/transformed` directory.

## 3. Text Chunking with Overlap Strategy

*   **Purpose**: To break down long contracts into smaller, manageable segments (chunks) suitable for embedding models, while preserving context across chunk boundaries.
*   **Method**: Utilized `langchain_text_splitters.RecursiveCharacterTextSplitter`.
    *   **Chunk Size**: Configured with a `chunk_size` of 1000 characters.
    *   **Overlap**: Implemented a `chunk_overlap` of 200 characters to maintain context.
    *   **Separators**: Defined specific separators (e.g., `\n\n`, `\n`, `. `, ` `) for intelligent splitting.
*   **Output**: Chunks for each contract were saved as JSON files in a `dataset/chunks` directory, each containing `contract_id`, `chunk_id`, `text`, and `num_chars`.
*   **Analysis**: Visualizations confirmed the distribution of chunk lengths and the effectiveness of the overlap strategy.

## 4. Chunk Embeddings and Vector Normalization

*   **Objective**: To convert text chunks into numerical vector representations (embeddings) that capture semantic meaning.
*   **Embedding Model**: Used the `SentenceTransformer` model "all-MiniLM-L6-v2" (a 384-dimensional model) for efficient and effective embedding generation.
*   **Process**: 
    *   All text chunks were extracted from the JSON files.
    *   The `embed_texts` function was applied to generate embeddings for each chunk.
    *   Embeddings were normalized to ensure consistent vector lengths.
*   **Storage**: The generated embeddings were saved as a NumPy array (`embeddings.npy`) and a pickle file (`chunks_with_embeddings.pkl`) for later use in vector search.

## 5. Semantic Search

*   **Functionality**: Implemented a `semantic_search` function to retrieve the most semantically similar contract chunks given a natural language query.
*   **Mechanism**: 
    *   The query is first embedded using the same `SentenceTransformer` model.
    *   Cosine similarity is calculated between the query embedding and all chunk embeddings.
    *   The top-k most similar chunks are returned, along with their similarity scores.
*   **Demonstration**: Successfully executed a semantic search query for "Confidentiality obligations in the contract" to retrieve relevant clauses.

---

**Next Steps**: Potentially integrate these components into a full-fledged application for contract analysis and clause extraction.
