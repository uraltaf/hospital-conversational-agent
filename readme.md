# Hospital System Chatbot: A LangChain-based RAG Solution

## Introduction

This project presents a sophisticated hospital system chatbot developed using LangChain, Neo4j, and Retrieval-Augmented Generation (RAG) techniques. The chatbot is designed to answer queries about a simulated hospital system, demonstrating the integration of AI with custom data sources and the implementation of graph databases. The solution showcases the deployment of AI chatbots using FastAPI and Streamlit, highlighting the practical application of advanced AI in real-world scenarios.

## System Architecture

The chatbot's architecture is built on several key components:

### 1. Data Model

The core data model is represented in a graph structure, as shown below:

![Data Model](images/data_model.png)

This model illustrates the relationships between various entities in the hospital system:

- Hospital: The central entity that employs physicians.
- Physician: Treats patients during visits and writes reviews.
- Patient: Has visits to the hospital.
- Visit: Represents a patient's hospital stay, treated by a physician.
- Payer: Covers the visit (likely an insurance entity).
- Review: Written by physicians about visits.

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

![Project Structure 1](images/project_structure_1.png)
![Project Structure 2](images/project_structure_2.png)

- `hospital_neo4j_etl/`: Contains scripts for data extraction, transformation, and loading into Neo4j.
- `chatbot_api/`: Houses the main application logic, including agents, chains, and models.
- `chatbot_frontend/`: Contains the Streamlit-based user interface.
- Docker configuration files for containerization and deployment.

## Implementation Details

### 1. Neo4j Integration
- A Neo4j AuraDB instance is set up to store and manage the hospital system's data.
- The graph database allows for efficient querying of complex relationships between entities.

### 2. LangChain RAG Implementation
- The chatbot uses LangChain to create a Retrieval-Augmented Generation system.
- It fetches both structured and unstructured data from Neo4j to provide context-aware responses.
- The RAG approach combines pre-trained language models with real-time data retrieval, ensuring up-to-date and accurate responses.

### 3. Agent Development
- The core logic is implemented in `hospital_rag_agent.py`, which defines the behavior and capabilities of the chatbot.
- Custom chains (e.g., `hospital_cypher_chain.py`, `hospital_review_chain.py`) are created to handle specific types of queries and data retrieval tasks.

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
   - Solution: Use of LangChain's RAG capabilities to combine pre-trained knowledge with real-time data.

5. **Scalability**
   - Challenge: Handling multiple concurrent users and large volumes of data.
   - Solution: Asynchronous operations, efficient database querying, and containerized deployment.

6. **Data Privacy and Security**
   - Challenge: Handling sensitive medical information securely.
   - Solution: Implementation of appropriate data access controls and encryption measures.
