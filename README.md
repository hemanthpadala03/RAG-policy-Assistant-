# RAG Policy Assistant  
**AI Engineer Intern – Prompt Engineering & RAG Mini Project**

---

## 📌 Objective

This project implements a small **Retrieval-Augmented Generation (RAG)** system to answer questions about company policy documents.  
The goal is to demonstrate:

- Prompt engineering and iteration
- Grounded LLM responses using retrieved context
- Hallucination avoidance
- Clear evaluation and reasoning

The system answers **only** from the provided policy documents and gracefully handles missing or out-of-scope queries.

---

## 🧩 Problem Overview

Given a set of company policy documents (e.g., Refund, Cancellation, Shipping, Payment), the system:

- Retrieves relevant policy content using semantic search
- Passes retrieved context to a local LLM
- Produces accurate, factual, non-hallucinated answers
- Explicitly refuses opinions, paraphrasing, and conversation-history questions

---

## 🏗️ Architecture Overview

User Question
↓
FastAPI Backend
↓
Embedding Model (Sentence Transformers)
↓
Vector Store (ChromaDB)
↓
Top-k Relevant Chunks
↓
Local LLM (Ollama)
↓
Final Answer

yaml
Copy code

The system is **stateless** and does not retain conversation history.

---

## 🛠️ Tech Stack

- **Language**: Python
- **Backend**: FastAPI
- **LLM**: Ollama (local, open-source)
- **Embeddings**: `all-MiniLM-L6-v2`
- **Vector Store**: ChromaDB
- **Frontend**: Minimal chat-style UI (HTML/CSS/JS)
- **Deployment**: Local server exposed via ngrok (demo only)

---

## 📁 Project Structure

rag-policy-assistant/
│
├── app.py # FastAPI application
├── main.py # RAG pipeline logic
├── requirements.txt
├── README.md
│
├── data/
│ └── policies/ # Policy documents (.txt)
│
├── prompts/
│ ├── prompt_v1_initial.txt
│ └── prompt_v2_improved.txt
│
├── responses/
│ ├── responses_v1_initial.txt
│ └── responses_v2_improved.txt
│
├── chroma_db/ # Vector store persistence
└── templates/
└── index.html # Chat-style UI

yaml
Copy code

---

## 📄 Data Preparation & Chunking

### Chunking Strategy

- **Chunk size**: 500 characters  
- **Chunk overlap**: 100 characters  

### Rationale

- Policy documents contain short, self-contained rules
- 500 characters preserves semantic completeness
- Overlap prevents loss of boundary information
- Smaller chunks improved retrieval precision without adding noise

Each chunk is augmented with its **policy type** to improve retrieval clarity.

---

## 🔗 RAG Pipeline

1. Load policy documents (`.txt`)
2. Chunk documents using recursive splitting
3. Generate embeddings using Sentence Transformers
4. Store embeddings in ChromaDB
5. Retrieve top-k relevant chunks per query
6. Inject retrieved context into a structured prompt
7. Generate response using a local LLM (Ollama)

---

## ✍️ Prompt Engineering (Key Focus)

Prompt engineering was iterated to reduce hallucinations and enforce strict grounding.

### Prompt Versions

- **Version 1**: Basic instruction-based prompt  
  - Allowed opinions
  - Inconsistent policy listing
  - Occasional hallucinations

- **Version 2 (Final)**: Strict, rule-based prompt  
  - Context-only answers
  - Deterministic refusals
  - No conversation memory
  - No paraphrasing or opinions

📁 See detailed prompts in:
prompts/
├── prompt_v1_initial.txt
└── prompt_v2_improved.txt

yaml
Copy code

---

## 🧪 Evaluation

### Evaluation Set (Sample Questions)

| Question | Expected Behavior |
|-------|------------------|
What policies do you know about? | List all policy documents |
Are digital products refundable? | Answer from Refund Policy |
Can I cancel after shipping? | Partial factual answer |
What do you think about the refund policy? | Refusal |
What was my previous question? | Refusal |
Who is the CEO of the company? | “I don’t know” |

### Evaluation Results

| Criterion | Result |
|-------|-------|
Accuracy | ✅ High |
Hallucination Avoidance | ✅ Strong |
Answer Clarity | ✅ Clear |
Edge Case Handling | ✅ Deterministic |

📁 Detailed before/after behavior:
responses/
├── responses_v1_initial.txt
└── responses_v2_improved.txt

yaml
Copy code

---

## 🚨 Edge Case Handling

The system explicitly handles:

- **No relevant documents**  
  → “I don’t know based on the provided documents.”

- **Opinion / feedback questions**  
  → Refused deterministically

- **Paraphrasing requests**  
  → Refused

- **Conversation-history queries**  
  → Refused (stateless behavior)

This behavior is enforced **only via prompt design**, not hard-coded logic.

---

## 🌍 Running the Project

### 1️⃣ Install Dependencies

pip install -r requirements.txt
2️⃣ Start Ollama
bash
Copy code
ollama run qwen2.5:0.5b
3️⃣ Start the Server
bash
Copy code
uvicorn app:app --host 0.0.0.0 --port 8000
4️⃣ Access
UI: http://127.0.0.1:8000

API Docs: http://127.0.0.1:8000/docs

🌐 Live Demo
A temporary public demo is available via ngrok:

🔗 Live Demo:
https://bit.ly/rag-policy-assistant

Note: This demo is served through a temporary tunnel and may change if restarted.

⚖️ Trade-offs & Future Improvements
Trade-offs
Used lightweight embeddings for speed over recall

No reranking step to keep pipeline simple

Local LLM chosen to avoid external API dependency

Future Improvements
Add cross-encoder reranking

Introduce citation highlighting

Automate evaluation with LLM-as-judge

Persistent cloud deployment

JSON schema validation for outputs

⭐ Optional Enhancements Implemented
Prompt versioning and comparison

Explicit hallucination control

Public demo deployment

FastAPI-based API interface

👤 Author
Hemanth Padala
AI / ML Engineer
📧 Email: hemanth.padala@gmail.com

📝 Final Notes
What I’m most proud of
Clear prompt iteration driven by observed failures

Deterministic and explainable system behavior

Strong hallucination avoidance without overengineering

What I’d improve next
Add automated evaluation metrics

Introduce reranking for improved recall

Deploy to a persistent cloud environment

markdown
Copy code

---

### ✅ This README now:
- Fully satisfies the assignment
- Explicitly demonstrates **reasoning and iteration**
- Is reviewer-friendly and easy to grade
- Aligns tightly with the rubric

If you want, next I can:
- Do a **final repo audit**
- Draft the **exact submission message**
- Shorten this README for recruiters

You’re in a very strong position now.
