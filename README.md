# 🤖 RAG API Project

> A Retrieval-Augmented Generation (RAG) API built with FastAPI, ChromaDB, Docker, Kubernetes, and GitHub Actions — part of the **NextWork DevOps × AI 4-Part Series**.

[![GitHub Repo](https://img.shields.io/badge/GitHub-Surraj70%2Frag--api--project-blue?logo=github)](https://github.com/Surraj70/rag-api-project)
[![Python](https://img.shields.io/badge/Python-3.9+-green?logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-latest-teal?logo=fastapi)](https://fastapi.tiangolo.com)
[![Docker](https://img.shields.io/badge/Docker-ready-blue?logo=docker)](https://docker.com)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-deployed-326CE5?logo=kubernetes)](https://kubernetes.io)

---

## 📖 What Is This Project?

Imagine you have a huge document — say, a guide about Kubernetes — and you want to ask it questions like *"What is Kubernetes?"* or *"How does container orchestration work?"* Instead of reading through everything manually, this project builds a **smart API** that reads the document for you and answers your questions intelligently.

This is called **RAG (Retrieval-Augmented Generation)**. Here's the simple idea:

1. 📄 **Load your document** — the app reads a knowledge base file (`k8s.txt`, which is a guide about Kubernetes).
2. 🔢 **Turn text into numbers (embeddings)** — each piece of text is converted into a list of numbers that capture its *meaning*. This is stored in a **vector database (ChromaDB)**.
3. ❓ **Ask a question** — when you send a question to the API, your question is also turned into numbers.
4. 🔍 **Find the closest match** — the app searches the database for text that has a similar meaning to your question.
5. 🤖 **Generate an answer** — the most relevant text is passed to an AI language model (LLM), which uses it to produce a clear answer.

This project is also fully containerized with **Docker**, deployed on **Kubernetes**, and tested automatically using **GitHub Actions CI/CD**.

---

## 🗂️ Project Structure

```
rag-api-project/
│
├── app.py                  # 🚀 Main FastAPI application — handles API requests
├── embed.py                # 🔢 Embeds the knowledge base (k8s.txt) into ChromaDB
├── semantic_test.py        # 🧪 Automated semantic tests (used in CI/CD pipeline)
├── k8s.txt                 # 📄 The knowledge base — a guide about Kubernetes
│
├── Dockerfile              # 🐳 Instructions to build a Docker container
├── deployment.yaml         # ☸️  Kubernetes Deployment config
├── service.yaml            # ☸️  Kubernetes Service config
├── k8s.txt                 # 📝 Kubernetes deployment notes/commands
│
├── db/                     # 💾 ChromaDB vector database files (auto-generated)
│
└── .github/
    └── workflows/          # ⚙️  GitHub Actions CI/CD pipeline definitions
```

### What Each File Does

| File | Purpose |
|------|---------|
| `app.py` | The heart of the project — a FastAPI web server that accepts questions and returns AI-generated answers using the RAG pipeline |
| `embed.py` | Reads `k8s.txt`, splits it into chunks, converts them to vector embeddings, and stores them in ChromaDB |
| `semantic_test.py` | Tests that verify the API returns *meaningful* answers (not just any text), used in automated CI/CD testing |
| `k8s.txt` | The knowledge source — contains information about Kubernetes that the RAG system learns from |
| `Dockerfile` | Packages the app and all its dependencies into a portable Docker container |
| `deployment.yaml` | Tells Kubernetes how many instances of the app to run and how to manage them |
| `service.yaml` | Tells Kubernetes how to expose the app so it can be accessed from outside the cluster |
| `.github/workflows/` | Automates testing every time code is pushed to GitHub |

---

## 🛠️ Tech Stack

| Tool | What It Does |
|------|--------------|
| **FastAPI** | Python framework to build the REST API |
| **ChromaDB** | Vector database to store and search text embeddings |
| **Sentence Transformers / Ollama** | Converts text to vector embeddings and generates answers |
| **Docker** | Packages the app into a portable container |
| **Kubernetes (k8s)** | Manages and scales Docker containers in production |
| **GitHub Actions** | Automatically runs tests on every code push |

---

## ✅ Prerequisites

Before you start, make sure you have the following installed:

- **Python 3.9+** → [Download here](https://www.python.org/downloads/)
- **pip** (comes with Python)
- **Git** → [Download here](https://git-scm.com/)
- **Docker** (for containerized deployment) → [Download here](https://docs.docker.com/get-docker/)
- **kubectl** (for Kubernetes deployment) → [Install guide](https://kubernetes.io/docs/tasks/tools/)
- A running **Kubernetes cluster** (local options: [Minikube](https://minikube.sigs.k8s.io/) or [Kind](https://kind.sigs.k8s.io/))

---

## 🚀 Setup Guide (Step by Step)

### Part 1 — Run Locally (No Docker)

#### Step 1: Clone the Repository

```bash
git clone https://github.com/Surraj70/rag-api-project.git
cd rag-api-project
```

#### Step 2: Create a Virtual Environment

A virtual environment keeps your project's dependencies separate from other Python projects.

```bash
# Create virtual environment
python -m venv venv

# Activate it (Mac/Linux)
source venv/bin/activate

# Activate it (Windows)
venv\Scripts\activate
```

You'll know it's active when you see `(venv)` at the start of your terminal prompt.

#### Step 3: Install Dependencies

```bash
pip install fastapi uvicorn chromadb sentence-transformers
```

> 💡 **What are these?**
> - `fastapi` — the web framework
> - `uvicorn` — the server that runs FastAPI
> - `chromadb` — the vector database
> - `sentence-transformers` — converts text to embeddings

#### Step 4: Embed the Knowledge Base

This step reads `k8s.txt`, converts it into vectors, and stores it in ChromaDB. **You only need to do this once.**

```bash
python embed.py
```

After running this, a `db/` folder will be created containing the ChromaDB vector store.

#### Step 5: Start the API Server

```bash
uvicorn app:app --reload
```

The API is now running at: **http://localhost:8000**

#### Step 6: Test the API

Open your browser and visit:

```
http://localhost:8000/docs
```

This opens the **interactive API documentation** (Swagger UI). You can send questions directly from here.

Or use `curl` from your terminal:

```bash
curl "http://localhost:8000/query?question=What+is+Kubernetes"
```

You should get a JSON response with an AI-generated answer about Kubernetes! 🎉

---

### Part 2 — Run with Docker

#### Step 1: Build the Docker Image

```bash
docker build -t rag-api .
```

This reads the `Dockerfile` and packages your app into a self-contained image.

#### Step 2: Run the Docker Container

```bash
docker run -p 8000:8000 rag-api
```

The `-p 8000:8000` flag maps port 8000 inside the container to port 8000 on your machine.

Visit **http://localhost:8000/docs** to use the API — same as before, but now it's running inside Docker!

#### Step 3 (Optional): Run in Mock LLM Mode

For testing without a real LLM, the app supports a mock mode that returns the retrieved documents directly instead of generating an AI response. This is useful for CI/CD pipelines:

```bash
docker run -p 8000:8000 -e USE_MOCK_LLM=1 rag-api
```

---

### Part 3 — Deploy to Kubernetes

#### Step 1: Make Sure Your Cluster Is Running

```bash
# If using Minikube
minikube start

# Verify the cluster is accessible
kubectl get nodes
```

#### Step 2: Push Your Docker Image to a Registry

Before deploying to Kubernetes, the image needs to be accessible. Push it to Docker Hub:

```bash
# Tag the image with your Docker Hub username
docker tag rag-api YOUR_DOCKERHUB_USERNAME/rag-api:latest

# Push to Docker Hub
docker push YOUR_DOCKERHUB_USERNAME/rag-api:latest
```

> 💡 Update `deployment.yaml` to reference your image name if needed.

#### Step 3: Apply the Kubernetes Configs

```bash
# Deploy the application
kubectl apply -f deployment.yaml

# Expose it with a Service
kubectl apply -f service.yaml
```

#### Step 4: Verify the Deployment

```bash
# Check that pods are running
kubectl get pods

# Check the service
kubectl get services
```

You should see your pods in a `Running` state.

#### Step 5: Access the API

```bash
# If using Minikube
minikube service rag-api-service --url
```

Copy the URL and open it in your browser with `/docs` at the end.

---

### Part 4 — GitHub Actions (CI/CD Automation)

This project includes a GitHub Actions workflow in `.github/workflows/` that automatically runs **semantic tests** every time you push code to the repository.

#### What the Pipeline Does

1. Triggers on every `git push`
2. Sets up Python environment
3. Installs dependencies
4. Starts the API in **mock LLM mode** (`USE_MOCK_LLM=1`)
5. Runs `semantic_test.py` to verify the API returns semantically meaningful answers

#### What Are Semantic Tests?

Regular tests check if the output *format* is correct (e.g., does the API return JSON?). **Semantic tests** go further — they check if the *meaning* is correct.

For example, when you ask *"What is Kubernetes?"*, a semantic test verifies that the answer includes key concepts like **"orchestration"** and **"containers"** — not just any random text.

#### Setting Up CI/CD on Your Fork

1. Fork the repository on GitHub
2. Push any change to trigger the workflow
3. Go to the **Actions** tab on GitHub to see the pipeline running

No extra configuration is needed — GitHub Actions runs everything automatically!

---

## 🌐 API Endpoints

Once the server is running, the following endpoints are available:

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Health check — confirms the API is running |
| `GET` | `/query?question=YOUR_QUESTION` | Ask a question and get a RAG-powered answer |
| `GET` | `/docs` | Interactive Swagger UI documentation |

### Example Request

```bash
curl "http://localhost:8000/query?question=What+is+Kubernetes"
```

### Example Response

```json
{
  "question": "What is Kubernetes?",
  "answer": "Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications..."
}
```

---

## 🧪 Running Tests Manually

To run the semantic tests locally:

```bash
# Start the API first (in a separate terminal)
USE_MOCK_LLM=1 uvicorn app:app --reload

# Then run the tests
python semantic_test.py
```

The tests will check that the API's responses contain the correct semantic meaning for various Kubernetes-related questions.

---

## 🔧 Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `USE_MOCK_LLM` | `0` | Set to `1` to return retrieved documents directly (no LLM call) — useful for testing |

---

## 🤔 Common Issues & Fixes

**Problem:** `ModuleNotFoundError` when running the app.

**Fix:** Make sure your virtual environment is activated and dependencies are installed:
```bash
source venv/bin/activate  # Mac/Linux
pip install fastapi uvicorn chromadb sentence-transformers
```

---

**Problem:** The `db/` folder doesn't exist and the API throws an error.

**Fix:** Run `embed.py` first to create the vector database:
```bash
python embed.py
```

---

**Problem:** Kubernetes pods are not starting (`CrashLoopBackOff`).

**Fix:** Check the pod logs for errors:
```bash
kubectl logs <pod-name>
```
