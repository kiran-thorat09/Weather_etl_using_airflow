# Weather Data Forecasting using Airflow

### 📌 Project Overview:

This project demonstrates a simple yet powerful ETL (Extract, Transform, Load) pipeline that automates the process of fetching weather data from the Open-Meteo API, transforming it using Python, orchestrating the workflow via Apache Airflow, and loading the processed data into a PostgreSQL database running inside a Docker container. The stored data can then be visualized or queried using DBeaver.

![image](https://github.com/user-attachments/assets/8f976492-6881-4504-b11a-55ea2a8ff53c)


### 🔄 ETL Flow Summary:

1)Extract
Real-time weather data is fetched from the Open-Meteo API using a Python script.

2)Transform
The raw JSON data is processed and cleaned using pandas (or plain Python), ensuring it's in a tabular format suitable for database storage.

3)Load
The transformed data is inserted into a PostgreSQL database. Airflow manages this step to ensure automation, retries, and proper logging.

4)Orchestrate with Airflow
Apache Airflow manages the entire ETL pipeline using a DAG (Directed Acyclic Graph). Each step (Extract → Transform → Load) is a task that runs inside the Airflow environment.

5)Dockerized Environment
Both Airflow and PostgreSQL run inside Docker containers, ensuring easy setup and consistency across environments.

6)Visualize in DBeaver
The final weather data can be explored and analyzed using the DBeaver GUI, which connects directly to the Dockerized PostgreSQL instance.

### 🧱 Tech Stack
1)Python – Data extraction & transformation

2)Open-Meteo API – Source of weather data

3)Apache Airflow – Workflow orchestration

4)PostgreSQL – Target data warehouse

5)Docker – Containerized deployment

6)DBeaver – Data exploration and visualization tool



# 📘 GraphRAG with FalkorDB
## 📌 Overview

This project implements a Graph-based Retrieval-Augmented Generation (GraphRAG) pipeline that processes PDF documents, extracts structured knowledge (entities and relationships), and stores it in a graph database (FalkorDB).

The system enables querying this graph to retrieve relevant context and generate answers using a language model.

## 🧠 Key Idea

Instead of relying on traditional vector-based retrieval, this project:

Extracts entities and relationships from text
Builds a knowledge graph
Uses the graph structure for context retrieval
Generates answers based on structured context


## 🏗️ Project Structure
GraphRAG_AWS/
│
├── data/
│   ├── raw_pdf/              # Input PDF documents
│   └── output/
│       └── pdf files         # Processed / extracted pdfs
│
├── src/
│   ├── preprocessing/
│   │   ├── pdf_loader.py     # Load and read PDFs
│   │   └── chunking.py       # Split text into chunks
│   │
│   ├── extraction/
│   │   └── extractor.py      # Entity & relationship extraction
│   │
│   ├── graph/
│   │   ├── graph_builder.py  # Build graph in FalkorDB
│   │   └── redis_connection.py
│   │
│   ├── retrieval/
│   │   └── retriever.py      # Retrieve and filter context
│   │
│   └── generation/
│       └── generator.py      # Generate answers using LLM
│
├── chatbot.py                # Chat interface
├── build_graph.py            # Graph construction script
└── requirements.txt


## ⚙️ Pipeline

### 1. 📄 Data Processing
File converter will convert doc and docx files to pdf.
converted PDF data is stored in:

data/output/
Used 2 PDFs for training purpose
Source location: data/
PDFs are loaded and converted into raw text
Text is divided into smaller chunks for better processing

### 2. 🧩 Knowledge Extraction
From each chunk:

Entities are identified
Relationships between entities are extracted


### 3. 🧱 Graph Construction
Entities are stored as nodes
Relationships are stored as edges
Graph is built in FalkorDB

Each node contains:

name
type
description (if available)
properties (if available)
document reference

### 4. 🔍 Retrieval
When a user asks a question:

Relevant entities and relationships are retrieved from the graph with the help of schema
Context is filtered to remove noise

### 5. 🤖 Answer Generation
Filtered context is passed to an LLM
Final answer is generated based on retrieved knowledge


## 🚀 How to Use

1. Install Dependencies
pip install -r requirements.txt

2. Run FalkorDB using Docker
docker run -p 6379:6379 -it --rm falkordb/falkordb
Starts FalkorDB locally
Default port: 6379

3. Build the Graph
python build_graph.py
Reads structured data from JSON
Creates nodes and relationships in FalkorDB

4. Run Chatbot
python chatbot.py
Enter queries
System retrieves graph-based context
Generates answers
🗄️ Data Format Example
{
  "doc_id": "doc_1",
  "chunk_id": "chunk_1",
  "chunk_text": "Sample text...",
  "entities": [...],
  "relationships": [...]
}


## ⚠️ Notes
The system relies on graph-based retrieval only
No embedding or vector-based search is used

Accuracy depends on:

Quality of extracted entities
Quality of relationships
Retrieval logic
🔧 Dependencies
Python
FalkorDB (via Docker)
Docker
LLM integration (used in extraction and generation)


## 🎯 Summary

This project demonstrates:

Converting unstructured PDFs into structured knowledge
Building a knowledge graph from extracted information
Using graph-based retrieval for question answering
