🧠 Build a RAG API with FastAPI (Project 1)

This project implements a local Retrieval-Augmented Generation (RAG) API using Python.
The API retrieves relevant information from a custom knowledge base and uses a local AI model to generate accurate, context-aware answers.

The entire system runs locally, without relying on paid cloud APIs or external inference services.

I built this project to understand how APIs, vector databases, and local language models can be composed into a production-style backend system.

🚀 What This Project Does

Exposes a REST API that accepts natural-language questions

Retrieves relevant information from a knowledge base using semantic search

Uses a local AI model to generate answers grounded in retrieved context

Returns responses in structured JSON format

Supports dynamic updates to the knowledge base without restarting the server

🧩 High-Level Architecture

The system follows the standard RAG (Retrieve → Augment → Generate) pattern:

A client sends a question to the API

The API performs semantic search over stored embeddings

Relevant document context is retrieved

The context and question are sent to a local language model

The model generates an answer grounded in the retrieved data

The API returns the response to the client

This design separates retrieval from generation, improving accuracy and reducing hallucination compared to standalone LLM prompts.

🛠️ Key Services & Technologies

FastAPI – API layer that exposes endpoints for querying and updating the knowledge base

Uvicorn – ASGI server that runs the FastAPI application

ChromaDB – Vector database used to store embeddings and perform semantic retrieval

Ollama – Local inference runtime for large language models

TinyLlama – Lightweight local language model used for answer generation

Python Virtual Environment – Isolates project dependencies and Python version

📁 Project Structure
nextwork-rag-api/
├── README.md        # Project documentation
├── app.py           # FastAPI application
├── embed.py         # Script to embed documents into ChromaDB
├── k8s.txt          # Example knowledge base document
├── db/              # Persistent ChromaDB storage
├── venv/            # Python virtual environment (not committed)
└── .gitignore
⚙️ Setup & Running the Project
1️⃣ Create and activate a virtual environment
py -3.13 -m venv venv
.\venv\Scripts\Activate
2️⃣ Install dependencies
pip install fastapi uvicorn chromadb ollama
3️⃣ Create embeddings for the knowledge base

Add content to k8s.txt, then run:

python embed.py

This converts the text into embeddings and stores them persistently in ChromaDB.

4️⃣ Run the API server
uvicorn app:app --reload

API base URL: http://127.0.0.1:8000

Interactive documentation (Swagger UI): http://127.0.0.1:8000/docs

Hot reloading allows code changes to take effect without manually restarting the server.

🔍 Querying the API

Example using PowerShell:

Invoke-RestMethod `
  -Uri "http://127.0.0.1:8000/query?q=What is Kubernetes?" `
  -Method Post

Example response:

{
  "answer": "Kubernetes is a container orchestration platform used to manage containers at scale."
}
➕ Adding Knowledge at Runtime

New knowledge can be added while the API is running:

Invoke-RestMethod `
  -Uri "http://127.0.0.1:8000/add?text=Pods are the smallest deployable unit in Kubernetes." `
  -Method Post

The content is embedded immediately and becomes available for subsequent queries without restarting the server.

⚠️ Limitations & Design Considerations

This implementation is stateful and intended for local development and learning

Model inference runs in-process via Ollama, which limits concurrency and scalability

The vector store and model runtime are coupled to the API process

In a production environment, these components would typically be:

containerized and deployed as separate services

protected with authentication and request validation

instrumented with logging and monitoring

optimized with document chunking and batching

🔮 Future Improvements

Document chunking for improved retrieval quality

Input validation and schema enforcement

Containerization with Docker

Deployment using Kubernetes

Automated rebuilds when knowledge base data changes

✅ Project Status

✔ Completed — local RAG API running successfully
✔ Supports querying and dynamic knowledge updates

📝 Notes

This project was built using guided steps and then extended to support runtime knowledge updates.
The focus was on understanding system composition, data flow, and operational behavior, rather than model training