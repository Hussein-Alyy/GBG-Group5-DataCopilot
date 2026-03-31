<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&duration=3000&pause=1000&color=00D4FF&center=true&vCenter=true&width=600&lines=Ultra+SQL+Agent+%F0%9F%A4%96;Chat+with+Your+Database;Natural+Language+%E2%86%92+SQL+Magic" alt="Typing SVG" />

<br/>

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![Azure OpenAI](https://img.shields.io/badge/Azure_OpenAI-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-Vector_Search-FF6B35?style=for-the-badge)

<br/>

> **Talk to your database in plain English — no SQL knowledge required.**
> Ultra SQL Agent translates natural language into precise SQL queries using GPT-4.1, RAG-powered few-shot examples, and a security-first architecture.

<br/>

[![MIT License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Tests](https://img.shields.io/badge/User_Tests-40%2B_Queries_Validated-success?style=flat-square)](docs/test_cases.md)
[![Status](https://img.shields.io/badge/Status-Active_Development-blue?style=flat-square)]()

</div>

---

## 📋 Table of Contents

- [✨ Overview](#-overview)
- [🚀 Features](#-features)
- [🏗️ Architecture](#️-architecture)
- [🧪 Test Coverage](#-test-coverage)
- [⚙️ Installation](#️-installation)
- [🔧 Configuration](#-configuration)
- [📁 Project Structure](#-project-structure)
- [🛡️ Security](#️-security)
- [👥 Team](#-team)

---

## ✨ Overview

**Ultra SQL Agent** is an intelligent, conversational data interface that enables users to query relational databases using everyday language. Powered by **Azure OpenAI GPT-4.1**, **LangChain**, and a **FAISS-based RAG pipeline**, it eliminates the barrier between non-technical users and complex data — no SQL expertise needed.

We built Ultra SQL Agent as part of our **GBG Academy AI training program**, and validated it across **40+ real-world queries** ranging from simple lookups to complex multi-table analytical questions.

---

## 🚀 Features

### 🧠 AI-Powered SQL Generation
- Converts natural language questions into accurate **PostgreSQL queries** in a single LLM call
- Uses **Pydantic-structured output** to guarantee response format reliability
- Handles complex queries: CTEs, window functions, subqueries, multi-table JOINs

### 🔍 RAG Few-Shot Engine
- **FAISS vector store** indexes question-SQL example pairs from `fewshots.json`
- At query time, retrieves the **3 most semantically similar** examples to guide generation
- Index is **cached on disk** — built once, loaded instantly on every run

### 🌍 Multilingual Understanding
- Detects query language automatically (**Arabic / English**)
- Responds in the **same language** the user wrote in
- Full Arabic RTL support in summarized responses

### 💬 Persistent Chat History
- All conversations stored in **SQLite** with session management
- Users can switch between past sessions and resume any conversation
- DataFrames are serialized as JSON and **restored perfectly** on reload

### 📊 Smart Result Pagination
- Queries returning **250+ rows** are automatically paginated
- Clear user notification with row counts and batch-fetch guidance
- Results rendered in **interactive Streamlit DataFrames**

### 🛡️ Security-First Design
- **Intent classifier** detects and blocks all `DROP`, `DELETE`, `UPDATE`, and `INSERT` attempts
- Enforces **SELECT-only** policy at the prompt layer
- No raw SQL injection possible — all queries go through structured validation

### 📈 LangSmith Observability
- Full **LangSmith tracing** integration for every LLM call
- Monitor latency, token usage, and chain performance in real time

---

## 🏗️ Architecture

```
User Input (Natural Language)
        │
        ▼
┌───────────────────────────────────────────────────────┐
│                   UltraSQLAgent                       │
│                                                       │
│  ┌─────────────────┐     ┌──────────────────────────┐ │
│  │  FAISS RAG       │────▶│  Azure OpenAI GPT-4.1    │ │
│  │  Few-Shot Store  │     │  (Single Unified Call)   │ │
│  └─────────────────┘     └──────────────┬───────────┘ │
│                                         │             │
│                          ┌──────────────▼───────────┐ │
│                          │  Pydantic UnifiedResponse │ │
│                          │  ┌──────────────────────┐ │ │
│                          │  │ intent: DB_QUERY      │ │ │
│                          │  │       GENERAL_CHAT    │ │ │
│                          │  │       SECURITY_VIOL.  │ │ │
│                          │  │ sql_query: SELECT ... │ │ │
│                          │  │ language: ar / en     │ │ │
│                          │  └──────────────────────┘ │ │
│                          └──────────────┬───────────┘ │
└─────────────────────────────────────────│─────────────┘
                                          │
           ┌──────────────────────────────┼─────────────────────────┐
           │                              │                         │
           ▼                              ▼                         ▼
  🛡️ SECURITY_VIOLATION           💬 GENERAL_CHAT            🗄️ DB_QUERY
  Block & warn user               Return LLM response        Execute on PostgreSQL
                                                             Summarize with GPT-4.1
                                                             Display DataFrame
```

---

## 🧪 Test Coverage

We validated Ultra SQL Agent against **40+ diverse queries** covering the full spectrum from basic lookups to advanced analytical questions:

### ✅ Basic Queries
| Query | Result |
|-------|--------|
| `list album names` | ✅ Passed |
| `list artist names` | ✅ Passed |
| `what's the number of artists?` | ✅ Passed — returned **275** |
| `can you show all customer names` | ✅ Passed |
| `show first 10 orders` | ✅ Passed |

### ✅ Filtering & Aggregation
| Query | Result |
|-------|--------|
| `Show customers from 'USA'` | ✅ Passed |
| `show all orders with total amount greater than 1000` | ✅ Passed |
| `What is the total amount of all orders` | ✅ Passed |
| `what is the total order amount per customer?` | ✅ Passed |
| `Show top 5 customers by total order amount` | ✅ Passed |

### ✅ Schema Exploration
| Query | Result |
|-------|--------|
| `what's the data about?` | ✅ Passed |
| `how many tables are in the data?` | ✅ Passed |
| `what are the columns of each table?` | ✅ Passed |
| `do customers ages exist in the data?` | ✅ Passed |
| `what are the genres of the music?` | ✅ Passed |

### ✅ Advanced Analytical Queries
| Query | Result |
|-------|--------|
| `Rank customers by total spending within each country — top 2 per country` | ✅ Passed (Window Function) |
| `Show the full management hierarchy from General Manager down` | ✅ Passed (Recursive CTE) |
| `Which sales support employee generated the highest revenue and what % of total?` | ✅ Passed (Subquery + ROUND) |
| `Show the most expensive track in each genre with album and artist name` | ✅ Passed (Multi-table JOIN) |
| `Which 3 months had the highest revenue growth vs previous month?` | ✅ Passed (LAG + DATE_TRUNC) |

---

## ⚙️ Installation

### Prerequisites

- Python 3.10+
- PostgreSQL database (or Chinook sample DB)
- Azure OpenAI resource with GPT-4.1 and text-embedding-3-small deployments

### 1. Clone the repository

```bash
git clone https://github.com/GBG-Group5-DataCopilot/ultra-sql-agent.git
cd ultra-sql-agent
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure environment variables

```bash
cp .env.example .env
# Edit .env with your credentials (see Configuration section)
```

### 4. Add your few-shot examples

Create a `fewshots.json` file:

```json
[
  {
    "naturalQuestion": "List all customers from USA",
    "sqlQuery": "SELECT \"FirstName\", \"LastName\" FROM \"Customer\" WHERE \"Country\" = 'USA'"
  }
]
```

### 5. Run the application

```bash
streamlit run APP.py
```

---

## 🔧 Configuration

Create a `.env` file in the project root:

```env
# Azure OpenAI
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_API_KEY=your-api-key
AZURE_OPENAI_API_VERSION=2025-01-01-preview
AZURE_OPENAI_CHAT_DEPLOYMENT=gpt-4.1
AZURE_OPENAI_EMBEDDING_DEPLOYMENT=text-embedding-3-small

# Database
DB_URL=postgresql://username:password@localhost:5432/chinook

# LangSmith Observability
LANGSMITH_API_KEY=your-langsmith-key
```

---

## 📁 Project Structure

```
ultra-sql-agent/
├── APP.py                  # Main Streamlit application
├── fewshots.json           # Few-shot Q&A examples for RAG
├── faiss_index/            # Cached FAISS vector index (auto-generated)
├── persistent_history.db   # SQLite conversation history (auto-generated)
├── deploy.py               # Deployment utilities
├── .env                    # Environment variables (not committed)
├── requirements.txt        # Python dependencies
└── data/
    ├── Album.csv
    ├── Artist.csv
    ├── Customer.csv
    ├── Employee.csv
    ├── Genre.csv
    ├── Invoice.csv
    ├── InvoiceLine.csv
    ├── MediaType.csv
    ├── Playlist.csv
    ├── PlaylistTrack.csv
    └── Track.csv
```

---

## 🛡️ Security

Ultra SQL Agent implements a **multi-layer security model**:

| Layer | Mechanism | Protection |
|-------|-----------|------------|
| **Intent Classification** | GPT-4.1 classifies every query before execution | Detects destructive intent |
| **Keyword Blocking** | System prompt rejects DROP/DELETE/UPDATE/INSERT | Prevents SQL injection via natural language |
| **SELECT Enforcement** | Only SELECT queries are forwarded to the database | Read-only guarantee |
| **Structured Output** | Pydantic validation on every LLM response | Prevents malformed SQL execution |

---

## 👥 Team

**GBG-Group5-DataCopilot** — Built with 🤝 collaboration and ☕ coffee

| Name | Role |
|------|------|
| **Hussein Aly Abd El-Bari** | Lead Developer & AI Engineer |
| **Mina Samy** | Backend & Database Engineer |
| **Amr Bahig** | LangChain & RAG Pipeline |
| **Nada Emad** | UI/UX & Streamlit Frontend |
| **Dana Ahmed** | Testing, QA & Documentation |

---

<div align="center">

**Built during the GBG Academy AI Program — Egypt 🇪🇬**

*"We don't just query data — we have a conversation with it."*

<br/>

⭐ If you find this project useful, please give it a star!

</div>
