# 🤖 Autonomous Data Analyst Agent

An advanced AI-powered agent that autonomously answers natural language questions by intelligently routing to the right tool — **SQL queries**, **document retrieval (RAG)**, or **Python code execution** — and combining them for complex multi-hop analysis.

> Built as a portfolio project showcasing end-to-end **NLP/LLM engineering**, **agentic AI**, and **full-stack data science**.

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![LangGraph](https://img.shields.io/badge/LangGraph-Agentic_AI-purple)
![Groq](https://img.shields.io/badge/LLM-Groq_Llama_3.3-orange)
![ChromaDB](https://img.shields.io/badge/VectorDB-ChromaDB-green)
![Streamlit](https://img.shields.io/badge/UI-Streamlit-red?logo=streamlit)

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 🧠 **Intelligent Routing** | LLM-based router classifies questions and picks the right tool(s) |
| 🗄️ **Text-to-SQL** | Converts natural language to SQL, queries SQLite, explains results |
| 📄 **RAG (Document Q&A)** | Retrieves context from uploaded PDFs using ChromaDB + sentence-transformers |
| 🐍 **Code Execution** | LLM generates & executes Python code in a sandboxed environment |
| 🔗 **Multi-hop Queries** | Chains multiple tools for complex analysis (e.g., query DB → visualize) |
| 📊 **Plotly Charts** | Auto-generates interactive visualizations |
| 💬 **Conversation Memory** | Maintains context across multi-turn conversations |
| 🔒 **Sandboxed Execution** | Blocks dangerous imports/builtins in code execution |
| 🔄 **Self-Correction** | Retries failed SQL/code with LLM-guided fixes (up to 3 attempts) |
| 🛡️ **Hallucination Guard** | RAG tool refuses to answer when no relevant context is found |

---

## 🏗️ Architecture

```
User Question
     │
     ▼
┌─────────────┐
│  LLM Router │  ← Classifies question type
└──────┬──────┘
       │
       ├──► SQL Tool    → SQLite (Superstore Sales DB)
       ├──► RAG Tool    → ChromaDB + PDFs
       ├──► Code Tool   → Sandboxed Python exec()
       ├──► Multi-hop   → Chains SQL → Code, etc.
       └──► Direct      → LLM answers directly
              │
              ▼
       ┌─────────────┐
       │ Synthesizer  │  ← Combines results into final answer
       └──────┬──────┘
              │
              ▼
         Final Answer + Charts + Sources
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| LLM | Groq API (`llama-3.3-70b-versatile`) |
| Agent Framework | LangGraph (StateGraph) |
| Vector Database | ChromaDB |
| Embeddings | sentence-transformers (`all-MiniLM-L6-v2`) |
| Database | SQLite |
| PDF Parsing | pdfplumber |
| Charts | Plotly |
| UI | Streamlit |
| Memory | LangChain ConversationBufferMemory |

---

## 📁 Project Structure

```
autonomous-data-analyst/
├── agent/
│   ├── __init__.py
│   ├── graph.py              # LangGraph agent orchestrator
│   ├── extras.py             # Schema, Insights, Confidence, Export, Guardrails
│   └── tools/
│       ├── __init__.py
│       ├── sql_tool.py       # Text-to-SQL with self-correction
│       ├── rag_tool.py       # RAG with ChromaDB + hallucination guard
│       └── code_tool.py      # Sandboxed Python execution
├── utils/
│   ├── __init__.py
│   ├── generate_data.py      # Superstore dataset generator
│   ├── db_loader.py          # CSV → SQLite loader
│   ├── generate_docs.py      # Sample PDF document generator
│   └── doc_loader.py         # PDF → ChromaDB pipeline
├── data/                     # Generated at runtime
│   ├── *.csv
│   ├── database.db
│   └── docs/*.pdf
├── vectorstore/              # ChromaDB persistent storage
├── app.py                    # Streamlit UI
├── test_phase1.py            # SQL tool tests (8/8 ✅)
├── test_phase2.py            # RAG tool tests (9/9 ✅)
├── test_phase3.py            # Code tool tests (8/8 ✅)
├── test_phase4.py            # Agent integration tests (8/8 ✅)
├── test_phase6.py            # Extra features tests (7/7 ✅)
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🚀 Quick Start

### 1. Clone & Install

```bash
git clone https://github.com/Nandini60/Autonomous-Data-Analyst-Agent.git
cd Autonomous-Data-Analyst-Agent
pip install -r requirements.txt
```

### 2. Set Up API Key

```bash
cp .env.example .env
# Edit .env and add your Groq API key
# Get a free key at: https://console.groq.com/keys
```

### 3. Generate Data

```bash
python -c "from utils.generate_data import generate_all; generate_all()"
python -c "from utils.db_loader import load_all_csvs; load_all_csvs()"
python -c "from utils.generate_docs import generate_all_docs; generate_all_docs()"
```

### 4. Run Tests

```bash
python test_phase1.py   # SQL Tool
python test_phase2.py   # RAG Tool
python test_phase3.py   # Code Tool
python test_phase4.py   # Agent Integration
python test_phase6.py   # Extra Features
```

### 5. Launch UI

```bash
streamlit run app.py
```

---

## 💡 Example Queries

| Query Type | Example |
|-----------|---------|
| **SQL** | "What are the top 5 products by profit?" |
| **SQL** | "Show total sales by region for 2023" |
| **RAG** | "What is the return policy for furniture?" |
| **RAG** | "What were the Q1 2024 revenue targets by region?" |
| **Code** | "Calculate compound interest on $10K at 8% for 5 years" |
| **Multi-hop** | "Get sales by category and create a bar chart" |
| **Direct** | "Hello! What can you help me with?" |

---

## 📊 Test Results

| Phase | Component | Tests | Status |
|-------|-----------|-------|--------|
| 1 | SQL Tool (Text-to-SQL) | 8/8 | ✅ All Passed |
| 2 | RAG Tool (Document Q&A) | 9/9 | ✅ All Passed |
| 3 | Code Tool (Python Exec) | 8/8 | ✅ All Passed |
| 4 | LangGraph Agent | 8/8 | ✅ All Passed |
| 6 | Extra Features | 7/7 | ✅ All Passed |

**Total: 40/40 tests passing** ✅

---

## 🛡️ Safety Features

- **PII Detection**: Detects emails, phone numbers, SSNs in queries
- **PII Redaction**: Automatically redacts sensitive data from outputs
- **Prompt Injection Guard**: Blocks common prompt injection attacks
- **Input Validation**: Query length limits and sanitization
- **SQL Injection Guard**: Blocks dangerous SQL operations (DROP, DELETE, etc.)
- **Sandboxed Code Execution**: Blocked dangerous Python imports (os, subprocess, etc.)

---

## 📜 License

This project is for educational and portfolio purposes.

---

## 🙏 Acknowledgments

- [Groq](https://groq.com/) for ultra-fast LLM inference
- [LangChain](https://langchain.com/) & [LangGraph](https://github.com/langchain-ai/langgraph) for the agent framework
- [ChromaDB](https://www.trychroma.com/) for vector storage
- [Sentence Transformers](https://www.sbert.net/) for embeddings
