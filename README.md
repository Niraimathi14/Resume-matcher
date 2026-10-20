# Resume-matcher
Python web app that scores your resume against a job description with live results, skill-gap tips and PDF upload. Uses weighted skill matching, cosine similarity and SQLite history.
# Resume–Job Match Analyzer

A Python web app that compares your resume with a job description and tells you how well they match. Paste or upload your resume, paste a job post, and the score updates live as you type. It also lists the skills you are missing, suggests how to learn them, and keeps a history of your results in SQLite.

Built with the Python standard library only (no frameworks), plus an HTML/CSS/JavaScript front end.

## Features

- **Live scoring:** the match score recalculates a moment after you stop typing.
- **Resume upload:** paste text, or upload a `.txt` or `.pdf` file.
- **Weighted skill matching:** skills in the job post are weighted by the wording around them. "Must have / required" counts 3, a normal mention counts 2, and "nice to have / preferred" counts 1.
- **Wording similarity:** a cosine-similarity score between resume and job text is blended with the skill score.
- **Gap report:** shows matched skills, missing skills, extra skills, and one practical tip for each missing skill.
- **Resume checklist:** checks for email, LinkedIn, GitHub, a projects section, certifications, and measurable results.
- **History in SQLite:** save results, view past runs, and see which skills you miss most often across job posts.
- **Command line mode** for quick checks from the terminal.

## How the score works

```
skill score = weighted matched skills / weighted required skills x 100
text score  = cosine similarity of resume words and job words x 100
final score = 0.75 x skill score + 0.25 x min(text score x 2.5, 100)
```

The tool recognises 20 skill groups (Java, Python, SQL, HTML, CSS, JavaScript, React, Spring Boot, REST APIs, OOP, Git, Docker, testing and more) and their common aliases, for example "oops" and "object-oriented" both count as OOP.

## Tech stack

| Part | Technology |
|------|------------|
| Backend | Python 3 (`http.server`, `sqlite3`, `re`, `dataclasses`) |
| Database | SQLite |
| Front end | HTML, CSS, JavaScript (fetch API) |
| PDF reading | `pypdf` (optional) |

## Getting started

**Requirements:** Python 3.9 or newer.

```bash
git clone https://github.com/Niraimathi14/resume-matcher.git
cd resume-matcher

# optional: only needed for PDF upload
pip install -r requirements.txt

python resume_matcher.py
```

Your browser opens at `http://localhost:8000`. If it does not, open that address yourself. Press `Ctrl+C` in the terminal to stop the app.

### Using the app

1. Paste your resume, or choose a `.txt` / `.pdf` file.
2. Paste a job description and, optionally, type a job title.
3. Read the score, matched and missing skills, and tips in the Result section.
4. Click **Save to history** to store the result in the History table.

### Command line mode

```bash
python resume_matcher.py cli --resume resume.txt --job job.txt --title "Java Developer"
python resume_matcher.py history
python resume_matcher.py serve --port 8001
```

## API endpoints

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/` | Web page |
| POST | `/api/analyze` | Score a resume against a job (`resume`, `job`, `title`, `save`) |
| POST | `/api/extract` | Extract text from an uploaded PDF |
| GET | `/api/history` | Recent results and most-missed skills |

## Project structure

```
resume-matcher/
├── resume_matcher.py   # backend, matching logic, SQLite and the web page
├── requirements.txt    # pypdf (optional, for PDF upload)
├── Procfile            # start command for hosting platforms
└── matches.db          # created automatically on first run
```

## Deploying

The app reads the `PORT` environment variable, so it runs on platforms such as Render or Railway.

- Build command: `pip install -r requirements.txt`
- Start command: `python resume_matcher.py`

Note: free hosting plans often reset the filesystem, so saved history may not persist there.

## Limitations

- Matching is keyword based, so it only knows the skills in its list. It is a study aid, not a real applicant tracking system.
- Scanned PDFs that contain only images cannot be read. Paste the text instead.
- History is shared by everyone who uses the same running copy of the app.

## Ideas for future work

- Add more skill groups and let users define their own.
- Use spaCy or sentence embeddings for smarter matching.
- Add user accounts so each person has a private history.
- Export the report as a PDF.

## Author

**Niraimathi M**: [GitHub](https://github.com/Niraimathi14) · [LinkedIn](https://linkedin.com/in/nirai-mathi)
