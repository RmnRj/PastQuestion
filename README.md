# PU Question Paper Analyzer

A static, JSON-driven web app for browsing university exam question papers, filtering questions, viewing repeated questions, and exploring syllabus topic frequency — all without any backend or framework.

---

## Table of Contents

1. [Opening the App](#1-opening-the-app)
2. [File Structure](#2-file-structure)
3. [Adding a New Subject](#3-adding-a-new-subject)
4. [JSON Formats](#4-json-formats)
   - [app-info.json](#41-app-infojson--app-settings)
   - [old_question_papers.json](#42-old_question_papersjson--question-bank)
   - [repeated_questions.json](#43-repeated_questionsjson--repeated-questions)
   - [syllabus.json](#44-syllabusjson--syllabus-with-frequency)
5. [Overview Stats Configuration](#5-overview-stats-configuration)
6. [Converting Question Papers to JSON Using AI](#6-converting-question-papers-to-json-using-ai)
7. [Tips & Notes](#7-tips--notes)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Opening the App

### Option A — App.bat (recommended, Windows)

Just double-click `App.bat`. It will:
- Check if Python is installed (and install it silently if not)
- Start a local server on port 8000
- Open `http://localhost:8000/app-page.html` in your browser automatically

### Option B — Python (manual)

```bash
cd path/to/PU-QuestionPaper_Analyzer/src
python -m http.server 8000
# Then open: http://localhost:8000/app-page.html
```

### Option C — Node.js (manual)

```bash
cd path/to/PU-QuestionPaper_Analyzer/src
npx http-server -p 8000
# Then open: http://localhost:8000/app-page.html
```

### Option D — VS Code Live Server

1. Install the **Live Server** extension in VS Code
2. Right-click `app-page.html` → **Open with Live Server**

> **Note:** The app uses `fetch()` to load JSON files, so it **cannot be opened by double-clicking** `app-page.html` directly.

---

## 2. File Structure

```
PU-QuestionPaper_Analyzer/
│
├── App.bat                     ← Windows launcher (auto-starts server)
├── LICENSE                     ← License file
├── README.md                   ← Documentation
│
└── src/
    ├── app-page.html               ← The app (do not rename)
    ├── app-info.json               ← App settings, overview config
    ├── pu-logo.png                 ← Optional logo
    │
    └── BE_COMPUTER/
        ├── Sem_VIII/
        │   ├── IS/
        │   │   ├── old_question_papers.json
        │   │   ├── repeated_questions.json
        │   │   ├── syllabus.json
        │   │   └── img/
        │   │       ├── 2023-S-6b.png
        │   │       └── ...
        │   ├── DSAP/
        │   ├── O&M/
        │   └── SPIT/
        │
        └── Sem_II/
            └── Thermal/
                ├── old_question_papers.json
                ├── repeated_questions.json
                ├── syllabus.json
                └── img/
                    ├── 2023-S-6b.png
                    └── ...
```

---

## 3. Adding a New Subject

### Step 1 — Clone the Repository

**Using HTTPS (recommended):**
```bash
git clone https://github.com/RmnRj/PU-QuestionPaper_Analyzer.git
```

**Or using SSH:**
```bash
git clone git@github.com:RmnRj/PU-QuestionPaper_Analyzer.git
```

### Step 2 — Create Folder Structure

Create folders following this pattern:
```
src/
└── BE_COMPUTER/
    └── Sem_II/
        └── Thermal/
            ├── old_question_papers.json
            ├── repeated_questions.json
            ├── syllabus.json
            └── img/
                ├── 2023-S-6b.png
                └── ...
```

**Folder naming rules:**
- Use meaningful names (e.g., `Thermal`, `IS`, `DSAP`)
- Match the path in `app-info.json`
- Create an `img/` subfolder for images

### Step 3 — Register in app-info.json

Add your subject to the `content` array:

```json
{
  "subject": "Thermal",
  "default": false,
  "option": "available",
  "folder": "BE_COMPUTER/Sem_II/Thermal"
}
```

**Note:** Just provide the folder path. The app will automatically load `old_question_papers.json`, `repeated_questions.json`, and `syllabus.json` from this folder.

### Step 4 — Create Required JSON Files

Each subject folder must contain exactly **3 files**:

1. **old_question_papers.json** — Question bank with all papers
2. **repeated_questions.json** — Frequently repeated questions
3. **syllabus.json** — Syllabus with topic frequency

See [Section 4](#4-json-formats) for detailed JSON structure.

### Step 5 — Run Locally

You need **Python** or **Node.js** installed to run a local server.

### Step 6 — Test and Submit Pull Request

1. Test the app locally to ensure your subject loads correctly
2. Verify all data displays properly and images/tables work
3. Commit your changes:
   ```bash
   git add .
   git commit -m "Add [Subject Name] question papers"
   git push origin main
   ```
4. Go to https://github.com/RmnRj/PU-QuestionPaper_Analyzer/pulls
5. Create a pull request from your fork
6. Include a description of what you added and confirm you tested it

---

## 4. JSON Formats

### 4.1 `app-info.json` — App Settings

```json
{
    "app_name":     "Question Analyzer",
    "app_ic":       "pu-logo.png",
    "university":   "PU",
    "program":      "BE_Computer",
    "course":       "SPIT",
    "course_code":  "2-0-0",
    "theme":        "light",
    "accent_color": "#2B5EA7",

    "tab_labels": {
        "questions": "Questions",
        "papers":    "Papers",
        "repeated":  "Repeated",
        "syllabus":  "Syllabus"
    },

    "overview": [
        { "label": "Papers",          "value": "auto:papers"    },
        { "label": "Questions",       "value": "auto:questions" },
        { "label": "Chapters",        "value": "auto:chapters"  },
        { "label": "Syllabus Topics", "value": "auto:topics"    },
        { "label": "Peak Repeats",    "value": "auto:peak"      },
        { "label": "Year Span",       "value": "auto:yearspan"  }
    ],
    
    "content": [
        {
            "subject": "IS",
            "default": true,
            "option": "available",
            "folder": "BE_COMPUTER/Sem_VIII/IS"
        }
    ]
}
```

#### Field Reference

| Field | Type | Description |
|---|---|---|
| `app_name` | string | Shown in header and browser tab |
| `app_ic` | string | Path to logo image (optional) |
| `university` | string | Shown in header subtitle |
| `program` | string | Shown in header subtitle |
| `course` | string | Shown in header badge |
| `course_code` | string | Shown in header badge |
| `accent_color` | hex string | Changes highlight color across the app |
| `tab_labels` | object | Rename any of the four tabs |
| `overview` | array | Stats bar configuration (see Section 5) |
| `content` | array | List of subjects with folder paths |

---

### 4.2 `old_question_papers.json` — Question Bank

```json
{
  "subject": "Thermal Engineering",
  "university": "Pokhara University",
  "programme": "BE — Mechanical Engineering",
  "level": "Bachelor",
  "full_marks": 100,
  "pass_marks": 45,
  "papers": [
    {
      "id": "2024_fall",
      "year": "2024",
      "semester": "Fall",
      "questions": [
        {
          "number": "1",
          "sub_no": "a",
          "marks": 7,
          "question": "Define thermodynamics...",
          "context": "Optional case study",
          "fig_url": "BE_COMPUTER/Sem_II/Thermal/img/2023-S-6b.png",
          "table_data": {
            "headers": ["Column 1", "Column 2"],
            "rows": [
              {"col1": "Value 1", "col2": "Value 2"}
            ]
          }
        }
      ]
    }
  ]
}
```

#### Question Fields

| Field | Type | Required | Description |
|---|---|---|---|
| `number` | string | Yes | Question number (e.g., "1", "7") |
| `sub_no` | string | Yes | Sub-part: `"a"`, `"b"`, `"or"` for alternates |
| `marks` | string/number | Yes | Marks for this question |
| `question` | string | Yes | Full question text |
| `context` | string | No | Case study or additional context |
| `fig_url` | string | No | Relative path to image from `src/` folder |
| `table_data` | object | No | Table with `headers` and `rows` arrays |

#### Image Rules

- Store images in `img/` subfolder within subject folder
- Use relative paths from `src/` folder
- Example: `BE_COMPUTER/Sem_II/Thermal/img/2023-S-6b.png`
- Supported formats: PNG, JPG, GIF, SVG

#### Table Rules

```json
"table_data": {
  "headers": ["Header 1", "Header 2", "Header 3"],
  "rows": [
    {"col1": "Value 1", "col2": "Value 2", "col3": "Value 3"},
    {"col1": "Value 4", "col2": "Value 5", "col3": "Value 6"}
  ]
}
```

---

### 4.3 `repeated_questions.json` — Repeated Questions

```json
{
  "repeated_questions": [
    {
      "question": "Define management and explain its functions.",
      "topic": "1.1 Meaning and concept of management",
      "freq": 20,
      "papers": ["2025_spring", "2024_fall", "2024_spring"]
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `question` | string | Representative question text |
| `topic` | string | Syllabus topic(s) this question maps to |
| `freq` | number | Number of papers this question appeared in |
| `papers` | array | List of paper IDs (matching `id` in `old_question_papers.json`) |

---

### 4.4 `syllabus.json` — Syllabus with Frequency

```json
{
  "course_title": "Thermal Engineering",
  "course_code": "ME 501",
  "total_hours": 45,
  "course_contents": [
    {
      "unit": 1,
      "title": "Introduction to Thermodynamics",
      "hours": 3,
      "topics": [
        { "topic": "1.1 Basic concepts", "frequency": 15 },
        { "topic": "1.2 First law", "frequency": 12 }
      ]
    }
  ],
  "references": [
    "Smith, Van Ness, Abbott - Introduction to Chemical Engineering Thermodynamics"
  ]
}
```

#### Frequency Color Classification

| Percentage of max frequency | Color | Label |
|---|---|---|
| ≥ 60% | Green | High |
| ≥ 30% | Amber | Mid |
| < 30% | Red | Low |

---

## 5. Overview Stats Configuration

The stats bar is configured in `app-info.json` under `"overview"`. Each item: `{ "label": "...", "value": "..." }`

### Auto-computed values

| Token | What it shows |
|---|---|
| `"auto:papers"` | Total number of exam papers |
| `"auto:questions"` | Total number of questions across all papers |
| `"auto:chapters"` | Number of units in the syllabus |
| `"auto:topics"` | Total topic count across all units |
| `"auto:peak"` | Highest repeat frequency (e.g. `20×`) |
| `"auto:yearspan"` | Year range (e.g. `2014–2025`) |
| `"auto:semesters"` | Available semesters (e.g. `Fall / Spring`) |

### Static values

```json
{ "label": "University", "value": "Pokhara University" },
{ "label": "Credits",    "value": "3" }
```

Remove the `"overview"` key entirely to hide the stats bar.

---

## 6. Converting Question Papers to JSON Using AI

Use **Claude Sonnet 4.6 (Extended Thinking)** or **Grok AI (Expert Mode)** to automatically convert question papers to JSON format.

### AI Models

- **Claude Sonnet 4.6 (Extended Thinking):** [Open Claude](https://claude.ai/)
- **Grok AI (Expert Mode):** [Open Grok](https://grok.com/)

### Step-by-Step Process

#### Step 1 — Copy the format example

Copy the JSON format from Section 4.2 (old_question_papers.json example).

#### Step 2 — Open AI model

Click one of the links above to open Claude or Grok in a new tab.

#### Step 3 — Upload and prompt

1. Upload your question paper (PDF, image, or paste text)
2. Paste this prompt:

```
Convert this question paper to JSON following this exact format:

[Paste the format example here]

Rules:
- Use "id": "YYYY_semester" (e.g. "2024_fall")
- Extract all questions with number, sub_no, marks, question text
- For OR questions use "sub_no": "or"
- For short notes (Q7) use "options" array
- Extract full question text accurately
```

#### Step 4 — Validate and save

1. Copy the AI-generated JSON
2. Save as `old_question_papers.json` in your subject folder

#### Step 5 — Create other files

Repeat the process for:
- `repeated_questions.json` — Use format from Section 4.3
- `syllabus.json` — Use format from Section 4.4

---

## 7. Tips & Notes

### Adding a new year's paper

1. Open `old_question_papers.json`, add a new entry at the top of `"papers"`:

```json
{
  "id": "2025_fall",
  "year": "2025",
  "semester": "Fall",
  "questions": [ ... ]
}
```

2. Update `freq` and `papers` arrays in `repeated_questions.json`
3. Update `frequency` counts in `syllabus.json`

### Frequency counting rule

- `frequency` = number of **papers** (not questions) in which the topic was asked
- Count once per paper even if asked multiple times in the same paper

### Changing the accent color

```json
"accent_color": "#B5451B"
```

### Renaming tabs

```json
"tab_labels": {
  "questions": "Q Bank",
  "papers":    "Past Papers",
  "repeated":  "Hot Topics",
  "syllabus":  "Chapters"
}
```

### Browser compatibility

Works in all modern browsers (Chrome, Firefox, Edge, Safari). No build step, no npm, no dependencies.

---

## 8. Troubleshooting

### App won't load / blank page

- Make sure you're using a local server (not opening `app-page.html` directly)
- Check browser console (F12) for errors
- Verify all JSON files are in the correct folders

### JSON syntax errors

- Common issues: missing commas, trailing commas, unescaped quotes
- Check browser console (F12) for specific error messages

### Images not loading

- Verify image paths are relative from `src/` folder
- Check that images exist in the `img/` subfolder
- Use forward slashes `/` in paths, not backslashes

### Port 8000 already in use

- Close any other servers running on port 8000
- Or use a different port: `python -m http.server 8001`

---

## Contributing

To add a new subject:

1. Fork the repository
2. Create a new folder following the structure
3. Add your JSON files (old_question_papers.json, repeated_questions.json, syllabus.json)
4. Register in `app-info.json`
5. Test locally to ensure everything works
6. Submit a pull request to https://github.com/RmnRj/PU-QuestionPaper_Analyzer/pulls

---

*Built for Pokhara University — Question Paper Analysis Tool*
