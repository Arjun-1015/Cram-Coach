```
 ██████╗██████╗  █████╗ ███╗   ███╗     ██████╗ ██████╗  █████╗  ██████╗██╗  ██╗
██╔════╝██╔══██╗██╔══██╗████╗ ████║    ██╔════╝██╔═══██╗██╔══██╗██╔════╝██║  ██║
██║     ██████╔╝███████║██╔████╔██║    ██║     ██║   ██║███████║██║     ███████║
██║     ██╔══██╗██╔══██║██║╚██╔╝██║    ██║     ██║   ██║██╔══██║██║     ██╔══██║
╚██████╗██║  ██║██║  ██║██║ ╚═╝ ██║    ╚██████╗╚██████╔╝██║  ██║╚██████╗██║  ██║
 ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝     ╚═════╝ ╚═════╝ ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝
                    E X A M   P A N I C   M O D E   E D I T I O N
```

<div align="center">

**AI-powered exam triage. Talk it, snap it, or upload it — get a study guide, flashcards, and a graded quiz back in seconds.**

[![Python](https://img.shields.io/badge/python-3.10%2B-blue)]()
[![Streamlit](https://img.shields.io/badge/streamlit-1.38.0-FF4B4B)]()
[![Gemini](https://img.shields.io/badge/AI-Gemini%202.0%20Flash-4285F4)]()
[![License](https://img.shields.io/badge/license-MIT-green)]()

[Live Demo](#-live-demo) • [Features](#-features) • [Installation](#-installation) • [Usage](#-usage-guide) • [Deployment](#-deployment) • [Troubleshooting](#-troubleshooting)

</div>

---

## `$ cat about.txt`

> **CramCoach** is a Streamlit + Google Gemini application built for one
> very specific moment: you have a few hours before an exam, a pile of
> messy notes, and no time to organize them yourself.

Feed it a **voice ramble**, a **photo of handwritten/printed notes**, or
an **uploaded PDF / DOCX / JPG**, and "The Cram Coach" — a blunt,
funny, high-urgency AI persona — reads or listens to it directly
(native Gemini multimodal understanding, no separate OCR or
transcription step) and turns it into:

- A **triaged study guide** prioritized by what's actually worth your remaining time
- An **editable flashcard deck**
- A **difficulty-tagged quiz**, auto-graded locally the moment you submit it
- A **readiness trend** tracked across every cram round in your session

```
[ VOICE RAMBLE ]   ─┐
[ PHOTO OF NOTES ]  ─┤
[ PDF UPLOAD ]      ─┼──▶ Gemini 2.0 Flash (Audio / Vision / Document) ──▶ Study Guide + Flashcards + Quiz ──▶ Auto-Graded Score
[ DOCX UPLOAD ]     ─┤        (DOCX text extracted locally first)
[ JPG UPLOAD ]      ─┘
```

---

## 🚀 Live Demo

```
Status: 200 LIVE
Host:   Streamlit Community Cloud
URL:    https://cramcoach.streamlit.app/
```
*(Replace with your actual deployment link before submission.)*

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎙️ **Voice-to-Study-Kit** | Record a rambled explanation via `st.audio_input` — Gemini listens natively, no transcription API needed |
| 📸 **Photo OCR (native)** | Snap handwritten or printed notes via `st.camera_input` — Gemini reads them directly, no OCR library needed |
| 📄 **File Upload** | Upload a `.pdf`, `.docx`, or `.jpg` — PDFs and JPGs go straight to Gemini natively; DOCX text is extracted locally via `python-docx` |
| ⏱️ **Time-Aware Triage** | Tell it how many hours you have left — the AI prioritizes differently for "2 hours" vs. "2 days" |
| 🎚️ **Difficulty Tiers** | Easy / Medium / Hard / Panic Mode — shapes both study guide depth and quiz difficulty |
| ✅ **Local Auto-Grading** | Quiz is graded instantly in Python — no extra API call, free to retake |
| 📊 **Weak-Spot Detection** | Grading identifies which difficulty tier you're missing most |
| 🗂️ **Editable Flashcards** | Full `st.data_editor` deck — add, edit, or delete cards before you study |
| 📈 **Readiness Trend** | Every graded cram round is logged and charted across your session |
| 🎭 **Coach Persona** | Witty, urgency-driven feedback — designed to actually be fun to demo |

---

## 🧱 Tech Stack

```
Frontend / App Framework .... Streamlit 1.38
AI Model ..................... Gemini 2.0 Flash (native Audio + Vision + PDF input, structured JSON output)
Data Handling ................ Pandas
Document Parsing ............. python-docx (DOCX text extraction only — PDFs/images go to Gemini natively)
Deployment .................... Streamlit Community Cloud / Render / Hugging Face Spaces
Version Control ............... Git + GitHub
```

---

## 📋 Prerequisites

Before you begin, make sure you have:

- **Python 3.10 or higher** — [download here](https://www.python.org/downloads/)
- **pip** (comes bundled with Python)
- **Git** — [download here](https://git-scm.com/downloads)
- A **free Gemini API key** — [get one here](https://aistudio.google.com/apikey)
- A webcam and microphone (only needed if you want to use the photo/voice input modes — file upload works without either)

Check your Python version:
```bash
python --version
# or on some systems:
python3 --version
```
You should see `Python 3.10.x` or higher.

---

## 🛠 Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/cram-coach.git
cd cram-coach
```

### 2. Create and activate a virtual environment

This keeps CramCoach's dependencies isolated from the rest of your system.

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows (PowerShell):**
```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

**Windows (Command Prompt):**
```cmd
python -m venv venv
venv\Scripts\activate.bat
```

You'll know it worked when your terminal prompt is prefixed with `(venv)`.

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

This installs:
```
streamlit==1.38.0
google-genai==0.3.0
pandas==2.2.2
python-docx==1.1.2
```

### 4. Get your Gemini API key

1. Visit [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Sign in with a Google account
3. Click **"Create API key"**
4. Copy the key — you'll paste it into the app's sidebar at runtime

> 🔒 **Never commit your API key to the repo.** CramCoach only asks for
> it inside the running app (via a password-masked input field held in
> browser session memory) — it is never written to disk, logged, or
> stored in any file.

### 5. Run the app locally

```bash
streamlit run app.py
```

Your default browser should open automatically to:
```
http://localhost:8501
```

If it doesn't open automatically, copy that URL into your browser manually.

### 6. Paste your API key and start cramming

Open the sidebar, paste your Gemini API key, set your hours-until-exam
and difficulty, pick an input mode, and hit **🔥 Cram It**.

---

## 📖 Usage Guide

### Step 1 — Configure your session (sidebar)
- Paste your **Gemini API key**
- Set **hours until your exam** (1–72)
- Pick a **difficulty**: Easy, Medium, Hard, or Panic Mode

### Step 2 — Choose an input mode
| Mode | Best for |
|---|---|
| 🎙️ Voice Ramble | Quickly explaining a concept out loud in your own words |
| 📸 Photo of Notes | Handwritten notes, whiteboard photos, textbook pages |
| 📄 Upload File | Existing PDFs (slides, scanned notes), Word docs, or saved images |

### Step 3 — Submit and review your Cram Kit
The app returns:
- A **pep talk** from the Coach (matched to your time pressure)
- A **triaged study guide** (high-yield bullet points only)
- An **editable flashcard deck**

### Step 4 — Take the quiz
Answer all 5 questions, click **✅ Grade Me**, and get:
- An instant percentage score
- Per-question review with the correct answer + explanation
- Your **weakest difficulty tier** flagged for extra review

### Step 5 — Track your readiness
Every graded round is logged in the **Cram History** table and charted
so you can watch your score trend upward across multiple rounds.

---

## 📁 Project Structure

```
cram-coach/
├── app.py                    # Main Streamlit application
├── requirements.txt          # Pinned Python dependencies
├── README.md                 # This file
├── ARCHITECTURE.md           # System design + Mermaid diagram
├── DESIGN_DOC.md             # Technical design document
└── .streamlit/
    └── config.toml           # Dark dashboard theme configuration
```

---

## ☁️ Deployment

### Option A — Streamlit Community Cloud (recommended, free)

1. Push this repo to your own public GitHub account
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Click **"New app"** → select your repo, branch (`main`), and `app.py` as the entry point
4. Click **Deploy**
5. Once live, copy the URL and paste it into the **Live Demo** section above and your GitHub repo description

> Note: your Gemini API key is entered by each user at runtime in the
> sidebar — you do **not** need to configure any secrets on Streamlit
> Cloud for this app to work, since no key is hardcoded server-side.

### Option B — Hugging Face Spaces

1. Create a new Space → SDK: **Streamlit**
2. Upload `app.py`, `requirements.txt`, and the `.streamlit/` folder
3. Space builds automatically and goes live at `https://huggingface.co/spaces/<you>/cram-coach`

### Option C — Render

1. Create a new **Web Service** on [render.com](https://render.com)
2. Connect your GitHub repo
3. Build command: `pip install -r requirements.txt`
4. Start command: `streamlit run app.py --server.port $PORT --server.address 0.0.0.0`

---

## 🧯 Troubleshooting

| Problem | Fix |
|---|---|
| `ModuleNotFoundError: No module named 'streamlit'` | Your virtual environment isn't activated, or dependencies weren't installed. Re-run `pip install -r requirements.txt`. |
| Microphone/camera not working | Browsers require **HTTPS** (or `localhost`) to grant mic/camera permissions. If deployed, confirm your URL uses `https://`. Also check your OS/browser has granted the site permission. |
| `"The AI response couldn't be parsed"` | Gemini occasionally returns malformed JSON under high load — just click submit again. |
| App loads but nothing happens after "Cram It" | Make sure you pasted a valid Gemini API key in the sidebar **before** submitting. |
| DOCX upload returns "no readable text" warning | The document may be scanned images with no real text layer — try the Photo of Notes mode instead, or export the doc pages as images. |
| `pip install` fails on Windows | Try `python -m pip install -r requirements.txt` instead, or ensure you're inside the activated `venv`. |
| Port `8501` already in use | Run `streamlit run app.py --server.port 8502` (or any free port). |

---

## ⚠️ Disclaimer

CramCoach generates AI-based study material for exam-prep support only.
It is not a substitute for your actual course materials, and answers
should always be double-checked against verified sources before an exam.

---

## 🤝 Contributing

This is a solo capstone project, but suggestions are welcome:

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/your-idea`)
3. Commit your changes (`git commit -m "Add: your idea"`)
4. Push and open a Pull Request

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.

---

## 👤 Author

Built by **`<your name>`** for the **MirAI School of Technology Capstone**
— Category B: EdTech & Campus Survival (`#8 Voice-Notes to Flashcards`,
customized into **Exam Panic Mode** with photo and multi-file upload support).

`⭐ Star this repo if the Cram Coach got you through finals.`
