# RAG Query API - Documentation

## 📘 Overview

This API implements a **Retrieval-Augmented Generation (RAG)** system that combines **semantic search** with **large language models (LLMs)** to deliver accurate, context-aware answers. It supports **multimodal queries** (text + image) and includes source attribution.

---

## 🚀 Key Features

- 🔍 **Semantic Search**: Uses vector embeddings to retrieve relevant context.
- 🖼️ **Multimodal Support**: Accepts both text and image queries.
- 🧠 **Knowledge Base**: Indexes Discourse forums and Markdown files.
- 📚 **Source Attribution**: Returns links and text snippets as citations.
- 💓 **Health Monitoring**: `/health` endpoint to monitor system status.

---

## 🔧 Installation

### Prerequisites

- Python 3.8+
- OpenAI API key (set as `API_KEY` environment variable)
- SQLite database (`knowledge_base.db`)

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/RAG-Query-API.git
   cd RAG-Query-API
Install dependencies:

bash
Copy
Edit
pip install fastapi uvicorn sqlite3 numpy aiohttp python-dotenv
Create a .env file:

env
Copy
Edit
API_KEY=your_openai_api_key_here
▶️ Usage
Running the API
bash
Copy
Edit
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
🔌 API Endpoints
1. POST /query
Submit a text/image-based question.

Request
json
Copy
Edit
{
  "question": "Your question here",
  "image": "base64_encoded_image_string" // Optional
}
Response
json
Copy
Edit
{
  "answer": "The generated answer",
  "links": [
    {
      "url": "https://source.url",
      "text": "Relevant text snippet"
    }
  ]
}
2. GET /health
Check server and DB status.

Response
json
Copy
Edit
{
  "status": "healthy",
  "database": "connected",
  "api_key_set": true,
  "discourse_chunks": 100,
  "markdown_chunks": 50,
  "discourse_embeddings": 100,
  "markdown_embeddings": 50
}
🧪 Example Requests
Text Query (cURL)
bash
Copy
Edit
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question":"What is the course structure for Machine Learning?"}'
Multimodal Query (Python)
python
Copy
Edit
import base64, requests

with open("diagram.jpg", "rb") as f:
    encoded = base64.b64encode(f.read()).decode("utf-8")

response = requests.post("http://localhost:8000/query", json={
    "question": "Explain this diagram",
    "image": encoded
})
print(response.json())
⚙️ Configuration
Environment Variables
Variable	Description	Default
API_KEY	OpenAI API Key	Required
DB_PATH	Path to SQLite DB	knowledge_base.db

Tuning Parameters
Parameter	Description	Default
SIMILARITY_THRESHOLD	Minimum similarity to include context	0.50
MAX_RESULTS	Max number of results to return	10
MAX_CONTEXT_CHUNKS	Max chunks per document	4

🗃️ Database Schema
discourse_chunks: Stores content + metadata + embeddings from Discourse.

markdown_chunks: Same structure for Markdown documentation.

❗ Error Handling
Common error types:

401 Unauthorized: Missing or invalid API_KEY

500 Internal Server Error: Processing or DB errors

🛠️ Troubleshooting
Issue	Resolution
Auth Errors	Check .env and API_KEY
DB Connection	Ensure knowledge_base.db exists and is populated
Slow Responses	Lower MAX_RESULTS, increase SIMILARITY_THRESHOLD
