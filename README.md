# Hospital System Chatbot: A LangChain-based RAG Solution with Groq and Llama3 ( Free to run: No paid API needed)

## Introduction

This project presents a hospital system chatbot developed using LangChain, Neo4j, and Retrieval-Augmented Generation (Graph-RAG). The chatbot is designed to answer queries about hospital system including reviews,wait times, visits, availaiblity,etc demonstrating the integration of AI with custom data sources and the implementation of graph databases. The chatbot can be deployed using FastAPI and Streamlit with code filess already uploaded,

## System Architecture

The chatbot's architecture is built on several key components:

### 1. Data Model

Data for this project can be found in the .env which has links to donwload it. It has been represented with different graph nodes and their relationship using Neo4j graph structure and uploaded to a Neo4j Aura Database.

Overal the graph shows the relationships between various entities in the hospital system:

- Hospital: The central entity that employs physicians.
- Physician: Treats patients during visits.
- Patient: Has visits to the hospital and writes reviews
- Visit: Represents a patient's hospital stay, treated by a physician.
- Payer: Covers the visit (likely an insurance entity).
- Review: Written by patients about visits to the hospital and physician.

### 2. Data Properties

Each entity in the graph has specific properties:

![Data Properties](images/data_properties.png)

- Visit Properties: Include admission and discharge dates, chief complaint, treatment description, room number, diagnosis, and test results.
- Physician Properties: Name, school, date of birth, graduation year, and salary.
- Hospital Properties: Name and state.
- Payer Properties: Name.
- Review Properties: Physician name, patient name, text content, and embedding.

### 3. Project Structure

The project is organized into several key directories and files:


- `hospital_neo4j_etl/`: Contains scripts for data extraction, transformation, and loading into Neo4j.
- `chatbot_api/`: Houses the main application logic, including agents, chains, and models.
- `chatbot_frontend/`: Contains the Streamlit-based user interface.
- Docker configuration files for containerization and deployment. and .env files to set your environment variables.

#Note: 
You don't need to load.dotenv() as this final application has docker which will handle it.
I have used LlamafileEmbeddings since I am using Llama 3.1 but you can choose any embedding model from HuggingFace.

## Implementation Details

### 1. Neo4j Integration
- A Neo4j AuraDB instance is set up to store and manage the hospital system's data.
- The graph database allows for efficient querying of complex relationships between entities.

### 2. LangChain RAG Implementation
- The chatbot uses LangChain to create a Graph Retrieval-Augmented Generation system.
- It fetches both structured and unstructured data from Neo4j to provide context-aware responses.
- The RAG approach combines pre-trained language models with real-time data retrieval, ensuring up-to-date and accurate responses.

### 3. Agent Development
- The core logic is implemented in `hospital_rag_agent.py`, which defines the behavior and capabilities of the chatbot.
- Custom chains (e.g., `hospital_cypher_chain.py`, `hospital_review_chain.py`) are created to handle specific types of queries and data retrieval tasks. The first one generates a cypher query for the Neo4j database using simply queries in natural language while the other chain presents review about the hospital and physician in natural language.

### 4. Query Processing
- The `hospital_rag_query.py` file in the models directory contains the logic for processing and understanding user queries.
- It includes natural language processing techniques to interpret user intent and formulate appropriate database queries.

### 5. Asynchronous Operations
- The `async_utils.py` file implements asynchronous operations for handling concurrent user requests and non-blocking database queries.

### 6. API Development
- FastAPI is used to create the backend API, exposing endpoints for the chatbot functionality.
- The API handles incoming requests, processes them through the LangChain agent, and returns responses.

### 7. Frontend Development
- A Streamlit-based user interface is created for easy interaction with the chatbot.
- The frontend communicates with the FastAPI backend to send user queries and display responses.

### 8. Containerization and Deployment
- Docker is used for containerizing the application, as evidenced by the Dockerfile and docker-compose.yml files.
- This setup ensures consistency across different deployment environments and simplifies the deployment process.

### 9. Data Pipeline
- The `hospital_bulk_csv_write.py` script in the ETL directory implements a data pipeline for bulk loading or updating the Neo4j database from CSV files.
- This allows for efficient data management and updates to the knowledge base.

## Technical Challenges and Solutions

1. **Graph Data Modeling**
   - Challenge: Representing complex hospital system relationships in a graph structure.
   - Solution: Careful design of the graph schema to capture all relevant entities and relationships.

2. **Efficient Data Retrieval**
   - Challenge: Quickly fetching relevant information from a large graph database.
   - Solution: Optimized Cypher queries and use of Neo4j's full-text search capabilities.

3. **Natural Language Understanding**
   - Challenge: Interpreting diverse user queries about the hospital system.
   - Solution: Implementation of sophisticated NLP techniques in the RAG model.

4. **Context-Aware Responses**
   - Challenge: Generating responses that consider both the user's query and the broader context.
   - Solution: Use of LangChain's RAG with Neo4j Graph capabilities to combine pre-trained knowledge with real-time data.

5. **Scalability**
   - Challenge: Handling multiple concurrent users and large volumes of data.
   - Solution: Asynchronous operations, efficient database querying, and containerized deployment.

6. **Data Privacy and Security**
   - Challenge: Handling sensitive medical information securely.
   - Solution: Implementation of appropriate data access controls and encryption measures.
