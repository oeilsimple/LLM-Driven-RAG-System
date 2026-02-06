# LLM-Based RAG System

## Overview

This project implements a Retrieval-Augmented Generation (RAG) system that combines real-time internet search with Large Language Model capabilities to provide accurate, context-aware answers to user queries. The system searches the web for relevant articles, processes the content, and uses vector similarity search to retrieve the most relevant information before generating responses through an LLM.

The application features a Flask-based REST API backend that handles search, content extraction, and LLM integration, paired with a Streamlit frontend for an intuitive user interface.

## Features

- **Real-time Web Search**: Automatically searches the internet for relevant articles based on user queries using SerpAPI
- **Intelligent Content Extraction**: Scrapes and processes article content, extracting meaningful text from web pages
- **Vector-based Retrieval**: Uses FAISS and sentence transformers to create embeddings and retrieve the most relevant content chunks
- **LLM-Powered Responses**: Generates contextual answers using Groq's LLaMA 3.3 70B model through LangChain
- **Source Attribution**: Provides sources for generated answers to ensure transparency and verifiability
- **Interactive Web Interface**: Clean and simple Streamlit-based UI for seamless user interaction
- **RESTful API**: Flask backend exposes endpoints for integration with other applications

## Tech Stack

### Backend
- **Flask** - Lightweight web framework for REST API
- **LangChain** - Framework for building LLM applications with retrieval chains
- **Groq API** - High-performance LLM inference (LLaMA 3.3 70B)
- **SerpAPI** - Web search API for retrieving relevant articles
- **BeautifulSoup4** - HTML parsing and content extraction
- **FAISS** - Vector similarity search for efficient retrieval
- **Sentence Transformers** - Text embedding generation

### Frontend
- **Streamlit** - Interactive web application framework

### Utilities
- **python-dotenv** - Environment variable management
- **Requests** - HTTP library for API calls
## How It Works

1. **User Input**: User enters a query through the Streamlit web interface
2. **API Request**: Query is sent to the Flask backend via POST request to `/query` endpoint
3. **Web Search**: Backend uses SerpAPI to search for relevant articles on the internet
4. **Content Extraction**: BeautifulSoup scrapes and extracts meaningful content from retrieved articles
5. **Vector Storage**: Content is chunked and converted to embeddings using sentence transformers, stored in FAISS vector database
6. **Retrieval**: Top-k most relevant chunks are retrieved based on semantic similarity to the query
7. **LLM Generation**: Retrieved context and query are passed to LLaMA 3.3 70B via LangChain's RetrievalQAWithSourcesChain
8. **Response**: Generated answer with source attribution is returned to the frontend and displayed to the user

## Project Structure

```
.
├── flask_app/
│   ├── app.py              # Flask REST API with /query endpoint
│   ├── utils.py            # Utility functions for search, scraping, and vector operations
│   └── __init__.py         # Package initialization
├── streamlit_app/
│   └── app.py              # Streamlit frontend interface
├── .env                    # Environment variables (API keys)
├── requirements.txt        # Python dependencies
└── README.md              # Project documentation
```

## Prerequisites

- Python 3.8 or higher
- SerpAPI account and API key
- Groq API account and API key

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <project-directory>
```

### 2. Create Virtual Environment

Using venv:
```bash
python -m venv venv
venv\Scripts\activate  # On Windows
source venv/bin/activate  # On macOS/Linux
```

Using conda:
```bash
conda create --name rag-system python=3.8
conda activate rag-system
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the root directory:

```env
SERP_API_KEY=your_serpapi_key_here
GROQ_API_KEY=your_groq_api_key_here
```

Replace the placeholder values with your actual API keys.

### 5. Run the Flask Backend

```bash
cd flask_app
python app.py
```

The Flask server will start on `http://localhost:5001`

### 6. Run the Streamlit Frontend

Open a new terminal and run:

```bash
cd streamlit_app
streamlit run app.py
```

The Streamlit app will open automatically in your browser at `http://localhost:8501`

## Usage

1. Open the Streamlit interface in your browser
2. Enter your query in the text input field
3. Click the "Search" button
4. View the generated answer along with source references

## API Endpoints

### POST /query

Processes a user query and returns an LLM-generated answer with sources.

**Request Body:**
```json
{
  "query": "Your question here"
}
```

**Response:**
```json
{
  "answer": "Generated answer text",
  "sources": ["source1.com", "source2.com"]
}
```

**Error Response:**
```json
{
  "error": "Error message"
}
```

## Configuration

- **LLM Model**: LLaMA 3.3 70B Versatile (configurable in `flask_app/app.py`)
- **Top-K Chunks**: 5 most relevant chunks (adjustable in `flask_app/app.py`)
- **Token Limit**: 1024 tokens for context (adjustable in `flask_app/app.py`)
- **Flask Port**: 5001 (configurable in `flask_app/app.py`)
- **Streamlit Port**: 8501 (default, configurable via Streamlit settings)

## Notes

- Ensure both Flask and Streamlit apps are running simultaneously for the system to work
- The `.env` file should never be committed to version control
- Vector store is created dynamically for each query to ensure fresh, relevant content
- API keys require active subscriptions to SerpAPI and Groq services