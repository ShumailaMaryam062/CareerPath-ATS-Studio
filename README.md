# CareerPath ATS Studio

CareerPath ATS Studio is a GenAI + Flask resume analysis web app designed for internship preparation (Backend Engineer Intern and Machine Learning Engineer Intern). It extracts resume text (PDF/DOCX), runs a multi-stage LLM pipeline (ATS scoring, improvements, skill gaps, career-path recommendations with RAG, and a learning roadmap), and exports a structured PDF report.

## Key Capabilities
- Upload PDF/DOCX resume and extract text
- ATS score (0–100) with breakdown, strengths, weaknesses, and summary
- Practical improvement suggestions (headline, formatting, content, keywords)
- Skill gap analysis (missing skills, critical gaps, priority skills)
- RAG-backed career path recommendations (retrieval from local `kb/` + LLM generation)
- Internship-focused learning roadmap (phases + projects)
- Downloadable PDF report (professional layout)
- Persistent analysis saved to `data/analysis_<analysis_id>.json`

## Architecture (High-Level)
1. Resume upload → text extraction
2. Create `analysis_id` and persist state
3. Stage pipeline:
   - `ats` (runs on upload)
   - `improvements`
   - `skill_gap`
   - `career_paths` (RAG retrieval + LLM)
   - `roadmap`
4. Export report as PDF using saved results

## Tech Stack
- Backend: Flask (Python)
- Frontend: HTML/CSS + Vanilla JavaScript
- Resume parsing: `pypdf`, `python-docx`
- LLM calls: `requests` (OpenAI-compatible chat/completions API)
- PDF: `reportlab`
- RAG (lightweight): keyword-based retrieval from local Markdown KB files

## API Endpoints
- `GET /` UI
- `POST /upload` Upload resume and run ATS stage
- `POST /analyze/<stage>` Run stage: `improvements`, `skill_gap`, `career_paths`, `roadmap`
- `GET /analysis/<analysis_id>` Fetch saved results JSON
- `GET /download-report/<analysis_id>.pdf` Download the full PDF report

## Repository Layout
- `app.py` Flask app + routes (student-friendly single file)
- `config.py` environment configuration
- `database/storage.py` persistence + in-memory cache
- `resume/parser.py` PDF/DOCX text extraction
- `prompts/system_prompts.py` system prompts per stage
- `prompts/stage_schemas.py` expected JSON keys + next-stage mapping
- `rag/retriever.py` KB retrieval from `kb/`
- `reports/pdf_report.py` PDF report generator
- `utils/` shared helpers (LLM client, JSON parsing, prompt cleanup, UUID validation)
- `templates/` HTML
- `static/` CSS + JS
- `kb/` role knowledge base (Markdown)
- `data/` saved analyses (auto-created)

## Setup & Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Create `.env` (example keys):
   - `GROK_API_KEY`
   - `GROK_API_URL` or `GROK_BASE_URL`
   - `GROK_MODEL`
3. Start server:
   ```bash
   python app.py
   ```
4. Open:
   - `http://127.0.0.1:5000/`
