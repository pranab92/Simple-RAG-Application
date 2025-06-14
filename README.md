# RAG Query API

A FastAPI-based Retrieval-Augmented Generation system that provides accurate answers with source attribution from Discourse forums and Markdown documentation.

## Features

- **Semantic Search**: Vector-based similarity search
- **Multimodal Support**: Handles both text and image queries
- **Knowledge Base**: SQLite database with Discourse and Markdown content
- **Source Attribution**: Returns answers with proper citations
- **Health Monitoring**: Built-in system status endpoint

## Installation

### Prerequisites

- Python 3.8+
- OpenAI API key
- SQLite

### Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   pip install fastapi uvicorn sqlite3 numpy aiohttp python-dotenv
