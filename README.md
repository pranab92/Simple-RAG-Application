## RAG Query API - Documentation ##
### Overview
This API implements a Retrieval-Augmented Generation (RAG) system that combines semantic search with large language models to provide accurate, context-aware answers to user queries. The system supports both text and image inputs (multimodal queries) and returns answers with proper source attribution.

Key Features
Semantic Search: Finds relevant content using vector embeddings

Multimodal Support: Processes both text and image queries

Knowledge Base: Stores and retrieves information from Discourse forums and Markdown documentation

Source Attribution: Provides citations for all answers

Health Monitoring: Includes a /health endpoint for system monitoring

Installation
Prerequisites
Python 3.8+

OpenAI API key (set as API_KEY environment variable)

SQLite database (knowledge_base.db)

Setup
Clone the repository

Install dependencies:

bash
pip install fastapi uvicorn sqlite3 numpy aiohttp python-dotenv
Create a .env file with your API key:

env
API_KEY=your_openai_api_key_here
Usage
Running the API
Start the server with:

bash
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
API Endpoints
1. Query Endpoint (POST /query)
Submit questions to the knowledge base.

Request Format:

json
{
  "question": "Your question here",
  "image": "base64_encoded_image_string"  // Optional
}
Response Format:

json
{
  "answer": "The generated answer",
  "links": [
    {
      "url": "https://source.url",
      "text": "Relevant text snippet"
    }
  ]
}
2. Health Check (GET /health)
Check system status and database connectivity.

Response Format:

json
{
  "status": "healthy",
  "database": "connected",
  "api_key_set": true,
  "discourse_chunks": 100,
  "markdown_chunks": 50,
  "discourse_embeddings": 100,
  "markdown_embeddings": 50
}
Example Requests
Text Query (cURL)
bash
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question":"What is the course structure for Machine Learning?"}'
Multimodal Query (Python)
python
import base64
import requests

with open("diagram.jpg", "rb") as image_file:
    encoded_image = base64.b64encode(image_file.read()).decode('utf-8')

response = requests.post(
    "http://localhost:8000/query",
    json={
        "question": "Explain this diagram",
        "image": encoded_image
    }
)
print(response.json())
Configuration
Environment Variables
Variable	Description	Default
API_KEY	OpenAI API key	Required
DB_PATH	Path to SQLite database	knowledge_base.db
Tuning Parameters
Parameter	Description	Default Value
SIMILARITY_THRESHOLD	Minimum similarity score for results	0.50
MAX_RESULTS	Maximum number of results to return	10
MAX_CONTEXT_CHUNKS	Maximum chunks per source document	4
Database Schema
The system uses two main tables:

discourse_chunks
Stores content from Discourse forums with metadata and embeddings.

markdown_chunks
Stores content from Markdown documentation with metadata and embeddings.

Error Handling
The API provides detailed error responses with logging. Common errors include:

401 Unauthorized - Missing or invalid API key

500 Internal Server Error - Database or processing errors

Troubleshooting
Authentication Errors:

Verify API_KEY is set in environment

Check logs for authentication failures

Database Issues:

Ensure knowledge_base.db exists and is accessible

Verify tables are created with proper schema

Performance Problems:

Reduce MAX_RESULTS or MAX_CONTEXT_CHUNKS

Increase SIMILARITY_THRESHOLD

License
MIT License
