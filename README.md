# Agentic Memory AI Assistant

An **Agentic Memory AI Assistant** that combines an LLM agent loop with persistent, semantic memory. The system can store user information, retrieve relevant memories, maintain session context, build a user profile, generate rolling summaries, and expose the agent through a React web interface.

## Overview

```text
React Frontend
      │
      ▼
FastAPI REST API
      │
      ▼
OpsAgent
 ┌────┼──────────────────────┐
 │    │                      │
 ▼    ▼                      ▼
Gemini  Memory Retrieval   Memory Write-back
       │                      │
       ▼                      ▼
     Milvus              Session/Profile
```

## Features

- Agentic chat loop using Google Gemini
- Persistent memory across sessions
- Semantic memory retrieval using vector embeddings
- Milvus / Milvus Lite vector storage
- User profile memory
- Session and episodic memory
- Rolling conversation summaries
- Automatic memory extraction and write-back
- Skill registry for operational/SRE-oriented tasks
- FastAPI REST backend
- React + Vite frontend
- Memory Inspector showing retrieved memories and agent-loop steps
- Session sidebar for managing conversations
- Free-tier-oriented Gemini configuration

## Tech Stack

### Backend

- Python
- FastAPI
- Google Gemini API
- `google-genai`
- Milvus / Milvus Lite
- PyMilvus
- Gemini Embeddings
- Pydantic

### Frontend

- React
- Vite
- Tailwind CSS
- JavaScript
- REST API

## Architecture

The backend is centered around `OpsAgent`.

A typical request follows this process:

1. Store the incoming user message.
2. Retrieve semantically relevant memories.
3. Load the user's persistent profile.
4. Load episodic/session context.
5. Match available skills from the skill registry.
6. Prune or summarize context when required.
7. Assemble the LLM prompt.
8. Generate a response with Gemini.
9. Write useful information back into memory.
10. Update the user profile and rolling summary.
11. Return the response and memory/debug information to the frontend.

## Project Structure

```text
agentic-memory-ai-assistant/
│
├── backend/
│   ├── agent.py
│   ├── main.py
│   ├── requirements.txt
│   ├── memory/
│   │   ├── __init__.py
│   │   ├── compression.py
│   │   ├── session_store.py
│   │   ├── skills.py
│   │   ├── user_profile.py
│   │   └── vector_store.py
│   └── skills_registry/
│       ├── SKILLS.md
│       ├── capacity_planning.md
│       ├── incident_triage.md
│       ├── monitoring_setup.md
│       └── rollback.md
│
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── index.html
│   └── src/
│       ├── App.jsx
│       ├── main.jsx
│       ├── index.css
│       └── components/
│           ├── ChatPanel.jsx
│           ├── MemoryInspector.jsx
│           └── SessionSidebar.jsx
│
├── .env.example
├── .gitignore
└── README.md
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/anantpathak26100-cpu/agentic-memory-ai-assistant.git
cd agentic-memory-ai-assistant
```

### 2. Create a Python virtual environment

From the project root:

```powershell
python -m venv venv
```

Activate it:

```powershell
.\venv\Scripts\Activate.ps1
```

### 3. Install backend dependencies

```powershell
pip install -r backend/requirements.txt
```

### 4. Configure the Gemini API key

Create a `.env` file in the project root:

```env
GEMINI_API_KEY=your_gemini_api_key_here
```

Never commit the real `.env` file.

The repository includes `.env.example` as a safe template.

### 5. Start the backend

From the project root:

```powershell
uvicorn backend.main:app --reload --port 8000
```

Backend:

```text
http://127.0.0.1:8000
```

Health check:

```text
http://127.0.0.1:8000/health
```

Interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

### 6. Install frontend dependencies

Open another terminal:

```powershell
cd frontend
npm install
```

### 7. Start the frontend

```powershell
npm run dev
```

The frontend runs on:

```text
http://localhost:5173
```

The Vite development server proxies API requests to the FastAPI backend on port `8000`.

## Environment Variables

```env
GEMINI_API_KEY=your_gemini_api_key_here
```

The application uses Gemini for generation and `gemini-embedding-001` for embeddings. The generation model is configured in `backend/agent.py`.

## Memory System

### Semantic Memory

Important facts can be converted into embeddings and stored in the vector database. This enables semantic retrieval instead of relying only on exact keyword matching.

### User Profile

Persistent user-level information can be maintained separately from individual conversations.

Example:

```text
Name: Anant
Study: CSE
```

### Session Memory

Conversation messages are stored per session so the agent can maintain context during a conversation.

### Rolling Summary

Long conversations can be compressed into summaries so the context remains manageable while preserving important information.

### Memory Write-back

After generating a response, the agent can extract useful information from the conversation and store it for future retrieval.

## Example

A user can tell the assistant:

```text
My name is Anant and I am a CSE student.
```

The agent can store this information.

In a later session, the user can ask:

```text
What is my name and what am I studying?
```

The assistant can retrieve the relevant memory and answer using persisted information.

## Agent Loop

The UI can expose internal processing information such as:

```text
stored_user_message
retrieved_memories
loaded_user_profile
no_skills_matched
assembled_prompt
llm_response_generated
updated_user_profile
rolling_summary_generated
```

This makes the project demonstrate an agentic workflow rather than only a basic chat interface.

## Skills Registry

The backend contains a skill registry for operational use cases including:

- Capacity planning
- Incident triage
- Monitoring setup
- Rollback

## Security

The repository ignores secrets and local runtime data such as:

```text
.env
venv/
node_modules/
__pycache__/
data/
*.db
*.sqlite
*.sqlite3
```

If an API key is accidentally committed to a public repository, rotate the key immediately.

## Screenshots

Add screenshots of the working application here.

Suggested screenshots:

- Main chat interface
- Memory Inspector showing retrieved memories
- Session sidebar
- Cross-session memory retrieval

## Future Improvements

- Memory importance scoring
- Memory deduplication and lifecycle management
- More skills and tool calling
- Authentication and multi-user support
- Streaming responses
- Production vector database deployment
- Automated tests
- Docker deployment
- GitHub Actions CI/CD
- Structured agent tracing and observability

## Why This Project

This project demonstrates practical concepts used in modern AI applications:

- LLM application development
- Agentic workflows
- Long-term memory
- Vector databases
- Semantic search
- REST API development
- Full-stack integration
- React frontend development
- Context management
- AI system debugging and observability

## Author

**Anant Pathak**

GitHub: https://github.com/anantpathak26100-cpu

---

Built as a full-stack demonstration of an AI assistant with persistent, semantic memory.
