# RAG (Retrieval-Augmented Generation) Pipeline

## Overview

RAG (Retrieval-Augmented Generation) is a technique that allows an LLM (Large Language Model) to answer user questions using relevant information retrieved from external documents, such as PDFs, instead of relying only on its pre-trained knowledge.

## Pipeline Steps

### 1. Upload PDF
- The user uploads a PDF document through the frontend.
- The PDF is sent to the backend for processing.

### 2. Extract and Store Document Content
- The backend extracts text from the uploaded PDF.
- The original PDF and/or extracted content can be stored on the server for future reference.

### 3. Split Content into Chunks
- The extracted text is divided into smaller sections called **chunks**.
- Chunking makes it easier to process and retrieve relevant information.
- Each chunk can optionally include metadata, such as the document name and page number.

### 4. Generate Vector Embeddings
- Each text chunk is converted into a numerical vector called an **embedding** using an embedding model.
- The embedding represents the semantic meaning of the text.

### 5. Store Embeddings in a Vector Database
- The generated embeddings are stored in a vector database along with their corresponding text chunks and metadata.
- This allows the system to search for relevant information efficiently.

Examples of vector databases:
- ChromaDB
- Pinecone
- FAISS
- Weaviate
- PostgreSQL with pgvector

### 6. Convert the User's Question into an Embedding
- When the user asks a question, the question is converted into a vector embedding using a compatible embedding model.
- This allows the system to compare the question with the stored document embeddings.

### 7. Perform Similarity Search
- The question embedding is compared with the embeddings stored in the vector database.
- The database retrieves the most relevant chunks based on their similarity to the question.
- A similarity metric, such as cosine similarity, can be used to rank the results.
- The system can retrieve the top-k chunks or apply a similarity threshold to filter out less relevant results.

### 8. Build Context and Generate the Response
- The retrieved chunks are combined to form the **context**.
- The context and the user's question are passed to the LLM through a prompt.
- The LLM uses the retrieved information to generate a relevant answer.
- The backend returns the generated answer to the frontend, optionally including source references.

## Key Concepts

### 1. Context

**Context is the relevant information provided to the LLM to help it answer the user's question accurately.**

**Example:**

Suppose a PDF contains information about cloud computing, and the user asks:

"What is virtualization?"

The system retrieves these chunks:

- Chunk 1: Virtualization allows multiple virtual machines to run on a single physical machine.
- Chunk 2: A hypervisor manages and allocates physical resources to virtual machines.

These retrieved chunks become the context supplied to the LLM.

The LLM uses this context to generate an answer grounded in the document.

### 2. Similarity Search

**Similarity search identifies the text chunks whose vector embeddings are most similar in meaning to the embedding of the user's question.**

**Example:**

User's question: "What is virtualization?"

| Document Chunk | Content | Relevance |
|---|---|---|
| Chunk A | Definition of virtualization | High |
| Chunk B | How hypervisors work | High |
| Chunk C | History of programming languages | Low |

The system retrieves the relevant chunks, such as A and B, and excludes unrelated chunks such as C.

Similarity scores are used to rank results; they do not necessarily represent exact percentages of semantic relevance.

## Complete Workflow

```text
User uploads PDF
       |
       v
Backend extracts text
       |
       v
Split text into chunks
       |
       v
Generate embeddings for chunks
       |
       v
Store chunks and embeddings
in the vector database
       |
       v
User asks a question
       |
       v
Generate question embedding
       |
       v
Similarity search in vector database
       |
       v
Retrieve relevant text chunks
       |
       v
Combine chunks into context
       |
       v
Pass context + question to LLM
       |
       v
Generate answer
       |
       v
Return response to the user
```

## Summary

RAG combines information retrieval with text generation:

- **Chunking:** Splits documents into manageable sections.
- **Embeddings:** Represent the semantic meaning of text as numerical vectors.
- **Vector database:** Stores embeddings and associated text for retrieval.
- **Similarity search:** Finds document chunks relevant to the user's question.
- **Context:** Supplies the retrieved information to the LLM.
- **Generation:** Produces an answer using the question and retrieved context.

**In simple words:** RAG finds the relevant information in your documents and gives it to an LLM so it can answer questions based on that information.
