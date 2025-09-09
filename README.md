### Project Overview
JobMate is a local Streamlit application designed to help job seekers optimize their resumes for Applicant Tracking Systems (ATS) and generate persuasive, role-aligned cold emails for recruiters. It has two sub-applications: an ATS Resume Scorer that evaluates keyword alignment and semantic similarity to a target role, and a Cold Email Generator that produces both a structured, recruiter-friendly email template and a longer AI-style pitch, all derived from the uploaded resume.

### Architecture and Technology
- App framework: Streamlit provides a fast, reactive UI that runs locally in your browser.
- PDF parsing: PyPDF2 extracts text from PDF resumes; works best with text-based (non-scanned) PDFs.
- Scoring and NLP: scikit-learn (TF-IDF and cosine similarity) powers semantic matching; lightweight heuristics detect achievements and skills without remote APIs.
- Role knowledge: A seed `data/roles.json` maps common roles (e.g., Software Engineer, Data Analyst) to typical keywords for coverage analysis.

### ATS Resume Scorer
The scorer analyzes two signals:
- Keyword coverage: It checks the resume for role-specific keywords (tokens/phrases) and computes the fraction matched. This reveals how ATS might parse alignment against job descriptions.
- Semantic similarity: Using TF-IDF vectors, it compares the resume text to a pseudo “role document” (keywords/role) via cosine similarity. This approximates content relevance beyond exact terms.

These are combined into a single ATS score (1–99). The app also lists missing keywords and offers actionable suggestions:
- Add or strengthen top missing terms
- Increase role-specific terminology
- Tailor the summary to mirror the role
- Use action verbs and quantify impact; keep to 1–2 pages

### Cold Email Generator
The generator produces two outputs:

1) Structured Template Output (your requested format)
- Subject and body fields are populated dynamically from the resume and UI inputs (Name, Title/Descriptor, Company, Recruiter, Product Signal, Observation, LinkedIn, Portfolio).
- Focus: concise, recruiter-friendly message with a strong, quantifiable highlight in the subject line.

2) AI-style (heuristic) Email Output
- Longer narrative using resume-derived achievements, inferred skills (based on role keywords), and estimated years of experience.
- Extracts quantified lines (e.g., percentages, dollars, metrics) and action-verb-led bullets to showcase impact.

### Heuristics and Data
- Achievements extraction: Finds bullet-like lines with metrics and action verbs.
- Skill inference: Counts mentions of role keywords in the resume to surface the top skills.
- Experience estimation: Uses year ranges to loosely infer years of experience.
- Role keyword data is simple and extensible—add or modify roles/keywords in `data/roles.json`.

### Privacy and Local-first Design
All processing runs locally on your machine. No third-party APIs are called, keeping resumes private and improving responsiveness.

### Extensibility Ideas
- Add OCR for scanned PDFs (e.g., Tesseract via pytesseract).
- Enrich role keywords with synonyms and weights.
- Import job descriptions to tailor ATS scoring and emails to specific postings.
- Support multiple email styles (formal, concise, technical deep-dive) and exporting to clipboard or email clients.

### Running the App
- Install dependencies: `py -m pip install -r requirements.txt`
- Start the app: `streamlit run app.py`
- Use the two tabs to score your resume and generate recruiter-ready emails tailored to your target role.
