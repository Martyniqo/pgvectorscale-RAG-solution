# Bank FAQ Retrieval System
### PostgreSQL + RAG

This project implements a system for answering bank-related FAQs by integrating **PostgreSQL** for data storage, **vector embeddings** for similarity search, and **RAG** (Retrieval-Augmented Generation) for high-quality language generation. The system is designed to handle large-scale question and answer pairs, with efficient vector search capabilities and integration with **OpenAI** to enhance the relevance and accuracy of the responses.

## Features
- **PostgreSQL with Vector Embeddings**: Store bank FAQs with vectorized embeddings for efficient similarity search, using the `pgvector` extension.
- **RAG Integration**: Combine retrieval with generative models to provide accurate, context-aware responses.
- **UUID from Time**: Leverage UUIDs based on insertion time for easy tracking and sorting of records.
- **Embeddings for Search**: Perform vector-based search using embeddings generated from question-answer pairs.
- **Poetry for Dependency Management**: Manage project dependencies and environments with **Poetry**, ensuring consistency across installations.

## Getting Started

### Prerequisites
- **Python 3.11+**
- **PostgreSQL** with the `pgvector` extension installed
- **Poetry** (for managing project dependencies)

### Setup Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/bank-faq-rag-system.git
   cd bank-faq-rag-system
   ```

2. Install dependencies using Poetry:
   ```bash
   poetry install
   ```

3. Create a `.env` file to store your OpenAI API key and other environment variables:
   ```
   OPENAI_API_KEY=your_openai_api_key_here
   DATABASE_URL=your_postgresql_database_url
   ```

4. Initialize the database and set up the vector tables:
   ```python
   from database.vector_store import VectorStore
   vec_store = VectorStore()
   vec_store.create_tables()
   vec_store.create_index()
   ```

5. Load the FAQ data into the system:
   ```python
   import pandas as pd
   df = pd.read_csv("data/BankFAQs.csv", sep=",")
   vec_store.upsert(df)
   ```

6. Query the vector database:
   ```python
   results = vec_store.search("How can I reset my bank account password?")
   print(results)
   ```

## Code Overview

### VectorStore Class

The `VectorStore` class manages all database interactions and vector operations. It uses **OpenAI** to generate embeddings and **Timescale Vector** for handling vector-based operations within PostgreSQL. Key methods include:

- `get_embedding(text)`: Generates an embedding for the provided text using OpenAI’s API.
- `create_tables()`: Sets up the necessary tables in the database.
- `create_index()`: Creates the `DiskAnnIndex` to speed up similarity searches.
- `upsert(df)`: Inserts or updates records in the database using a pandas DataFrame.
- `search(query_text)`: Queries the database for similar embeddings based on input text.
- `delete(ids, metadata_filter, delete_all)`: Deletes records based on ID, metadata, or deletes all records.

## License

This project is licensed under the MIT License.