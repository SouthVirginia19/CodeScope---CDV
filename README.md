# 🔍 CodeScope

**Interactive code dependency visualizer with AI-powered analysis.**

Point CodeScope at any Python project — a local folder or a GitHub URL — and get an interactive dependency graph, hotspot ranking, security scan, health score, and AI-generated explanations. Runs fully locally, no cloud required (except optional AI features).

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.119-009688?style=flat-square&logo=fastapi&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Cytoscape.js](https://img.shields.io/badge/Cytoscape.js-3.30-F7A800?style=flat-square&logo=cytoscape&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0-D71F00?style=flat-square&logo=sqlalchemy&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-3-003B57?style=flat-square&logo=sqlite&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)

---

## 📸 Screenshots

> Add screenshots here after your first run. Drag PNGs into the GitHub README editor — they will upload and insert the correct links automatically.

<!-- ![Dependency graph](docs/graph.png) -->
<!-- ![Health score and security](docs/health.png) -->

---

## ✨ Features

### 🔍 Analysis
- 🕸 **Interactive dependency graph** — files as nodes, imports as edges, powered by Cytoscape.js
- 🔥 **Hotspot ranking** — files ranked by `complexity × (1 + dependents)` to surface technical debt
- 🎯 **Blast radius** — click any node to see every file that transitively depends on it
- ⚠️ **Circular import detection** — DFS-based cycle finding with highlighted paths
- 🌿 **Orphan file detection** — files nobody imports and that aren't entry points
- 🔒 **Security scanner** — finds secrets (API keys, tokens, private keys) and dangerous calls (`eval`, `exec`, `os.system`, `pickle.load`, …)
- ❤️ **Health score** — 0–100 with transparent breakdown (cycles, complexity, orphans, security)

### ✨ AI (optional)
- 🧠 **Project-wide review** — AI analyzes metrics, hotspots, cycles, and security findings
- 📝 **Per-file explanations** — click any node and get a structured explanation (purpose, components, pitfalls)
- 💾 **Saved explanations** — AI answers are stored per analysis and included in reports

### 📊 Workflow
- 📄 **Markdown report export** — full project report with metrics, hotspots, security, AI notes
- 🔀 **Diff analysis** — compare two analyses and see what changed (files, edges, health, security)
- 📜 **Analysis history** — every run stored in SQLite
- 🌐 **Git URL support** — analyze any public repo: `https://github.com/tiangolo/fastapi`
- 🖼 **Export PNG** — save the graph as an image
- 🎨 **Four layouts** — Force, Cluster, Hierarchy, Circle, Grid
- ⌨️ **Zero-config** — works offline; AI features require an OpenRouter key

---

## 🛠 Tech Stack

**Backend**
- Python 3.11+
- FastAPI
- SQLAlchemy 2 + SQLite
- Pydantic v2 + pydantic-settings
- Python `ast` for parsing
- OpenRouter API (via `openai` SDK) for optional AI features
- git (subprocess) for URL cloning

**Frontend**
- Vanilla JavaScript (ES6+)
- Cytoscape.js for graph visualization
- Plain CSS with custom properties
- No build step, no framework

---

## 📁 Project Structure

```text
codescope/
|-- backend/
|   |-- app/
|   |   |-- __init__.py
|   |   |-- config.py                # settings from .env
|   |   |-- database.py              # SQLAlchemy setup
|   |   |-- models.py                # Analysis model
|   |   |-- schemas.py               # Pydantic schemas
|   |   |-- main.py                  # FastAPI entry point
|   |   |-- analyzer/
|   |   |   |-- __init__.py          # analyze_project() entry
|   |   |   |-- parser.py            # AST parsing + metrics
|   |   |   |-- graph.py             # dependency graph, cycles, health
|   |   |   |-- security.py          # secrets + dangerous calls
|   |   |   `-- fetcher.py           # git clone for URL analysis
|   |   `-- api/
|   |       |-- __init__.py
|   |       |-- analysis.py          # /api/analyze, /api/analyses, compare
|   |       |-- ai.py                # /api/ai/explain, /api/ai/project-review
|   |       `-- profile.py           # GitHub profile proxy
|   |-- requirements.txt
|   `-- .env.example
|-- frontend/
|   |-- index.html
|   |-- style.css
|   |-- app.js
|   `-- cytoscape.min.js
|-- .gitignore
`-- README.md
```
🚀 Quick Start
Requirements

    Python 3.11+ — https://www.python.org/downloads/

    Git — https://git-scm.com/

    (Optional) OpenRouter API key — https://openrouter.ai/keys

1. Clone the repository
bash

git clone https://github.com/SouthVirginia19/CodeScope---CDV

2. Create a virtual environment
bash

python -m venv .venv

# Windows (PowerShell)
.\.venv\Scripts\python.exe -m pip install --upgrade pip

# Linux / macOS
source .venv/bin/activate

    We call python.exe from the venv directly instead of activating — this works the same on Windows, Linux, and macOS, and avoids PowerShell execution-policy issues.

3. Install dependencies
bash

# Windows
.\.venv\Scripts\python.exe -m pip install -r requirements.txt

# Linux / macOS
./.venv/bin/python -m pip install -r requirements.txt

4. Configure environment
bash

# Windows
Copy-Item .env.example .env

# Linux / macOS
cp .env.example .env

Edit backend/.env:
env

# Required
DATABASE_URL=sqlite:///./codescope.db

# Optional — AI features (get a key at https://openrouter.ai/keys)
AI_PROVIDER=openrouter
OPENROUTER_API_KEY=sk-or-v1-your-key-here
AI_MODEL=nvidia/nemotron-3-ultra-550b-a55b:free

    Free models available at https://openrouter.ai/models?max_price=0 — replace AI_MODEL with any of their IDs.

5. Run the server
bash

# Windows
.\.venv\Scripts\python.exe -m uvicorn app.main:app --reload --port 8000

# Linux / macOS
./.venv/bin/python -m uvicorn app.main:app --reload --port 8000

Expected output:
text

INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete.

6. Open in your browser

👉 http://127.0.0.1:8000
🎮 Usage
Analyze a project

Local folder:
text

E:/my-python-project

Git URL:
text

https://github.com/tiangolo/fastapi

Click ⚡ Analyze.
Explore the graph
Action	Result
Scroll	Zoom in/out
Drag background	Pan
Drag a node	Reposition
Click a node	Open detail panel
🎯 Blast radius	Highlight all transitive dependents
✨ Explain with AI	Get a structured explanation from the AI
Read the metrics
Card	Meaning
❤️ Health	0–100 score with A+ … F grade
📁 Files	Total .py files
📝 Lines of code	Non-empty lines
🔗 Dependencies	Import edges between files
🔧 Functions	Function definitions
🏛 Classes	Class definitions
📊 Avg complexity	Mean cyclomatic complexity
📈 Max complexity	Worst file
⚠ Cycles	Circular import count
🌿 Orphans	Dead-code candidates
🔒 Security	Total security findings
Health score

The score starts at 100 and is reduced by:
Issue	Penalty
Each circular import	up to −30 total
Max complexity > 20	−20
Max complexity 16–20	−10
Avg complexity > 8	−15
Orphan ratio > 30%	−25
Each high-severity security finding	up to −25 total

The Health breakdown panel below the graph shows exactly what moved the score and by how much.
Keyboard shortcuts
Key	Action
Escape	Clear blast radius / close detail panel
🔌 API Endpoints
Method	Endpoint	Description
GET	/api/health	Health check
POST	/api/analyze	Analyze a project. Body: {"path": "..."} — local path or git URL
GET	/api/analyses	List stored analyses
GET	/api/analyses/{id}	One analysis with full graph
GET	/api/analyses/compare?a=1&b=2	Diff two analyses
DELETE	/api/analyses/{id}	Delete an analysis
POST	/api/ai/explain	Explain a single file. Body: {path, root_path, analysis_id?}
POST	/api/ai/project-review	Review the whole project. Body: {analysis_id}
GET	/api/profile	GitHub profile proxy (query: username)
Example
bash

curl -X POST http://127.0.0.1:8000/api/analyze \
  -H "Content-Type: application/json" \
  -d '{"path": "https://github.com/tiangolo/fastapi"}'

Response (truncated):
json

{
  "id": 1,
  "name": "fastapi",
  "metrics": {
    "total_files": 1138,
    "total_loc": 97219,
    "total_edges": 1618,
    "avg_complexity": 2.84,
    "max_complexity": 351,
    "cycles": [["fastapi/utils.py", "fastapi/routing.py", "fastapi/utils.py"]],
    "orphans": ["...", "..."],
    "security": { "summary": { "high": 0, "medium": 27, "low": 0 } },
    "health": { "score": 15, "grade": "F", "breakdown": [...] }
  },
  "hotspots": [ { "file": "fastapi/routing.py", "complexity": 351, ... } ]
}

🧠 How it works

    Fetch — for URLs, git clone --depth=1 into a temp folder

    Scan — walks the tree, skipping .venv, __pycache__, node_modules, etc.

    Parse — uses Python's built-in ast to extract imports, functions, classes, LOC

    Complexity — counts branch nodes (if, for, while, and, or, except) + 1

    Resolve imports — handles both absolute (from app.config import X) and relative (from .config import X) imports by walking the module hierarchy

    Build graph — modules become nodes, imports become directed edges

    Detect cycles — DFS with white/gray/black coloring

    Security scan — regex for secrets, AST for dangerous calls

    Rank hotspots — complexity × (1 + dependents), sorted descending

    Health score — additive penalties; the breakdown is stored for display

    Store — the whole result is serialized to SQLite; AI explanations are attached to the same row

    AI — sends metrics + top hotspots to OpenRouter for project review, or file source for per-file explanation

⚙️ Configuration

All settings live in backend/.env:
Variable	Default	Description
DATABASE_URL	sqlite:///./codescope.db	SQLAlchemy connection string
AI_PROVIDER	openrouter	Currently only openrouter is implemented
OPENROUTER_API_KEY	""	Get one at https://openrouter.ai/keys
AI_MODEL	nvidia/nemotron-3-ultra-550b-a55b:free	Any model ID from OpenRouter
OLLAMA_URL	http://localhost:11434	Reserved for future Ollama support
🗺 Roadmap

    ☑

    Python project analysis
    ☑

    Interactive dependency graph
    ☑

    Hotspot ranking
    ☑

    Blast radius analysis
    ☑

    Circular import detection
    ☑

    Orphan file detection
    ☑

    Security scanner (secrets + dangerous calls)
    ☑

    Health score with breakdown
    ☑

    Diff analysis between runs
    ☑

    Markdown report export
    ☑

    AI per-file explanations and project review
    ☑

    Git URL support
    □

    📦 JavaScript / TypeScript support (tree-sitter)
    □

    🦀 Rust support
    □

    🐘 PHP support
    □

    🧠 MCP server for AI agents
    □

    🌳 Git history analysis (per-commit trends)
    □

    🌓 Light theme toggle
    □

    🚀 Deploy to Railway / Fly.io

🤝 Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change.

    Fork the repository

    Create a branch: git checkout -b feature/amazing-feature

    Commit: git commit -m "Add amazing feature"

    Push: git push origin feature/amazing-feature

    Open a Pull Request

📄 License

Released under the MIT License. See LICENSE for details.
👤 Author

SouthVirginia19

GitHub: @SouthVirginia19

⭐ If this project was useful, consider giving it a star!
