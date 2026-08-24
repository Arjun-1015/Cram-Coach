```
 ██████╗██████╗  █████╗ ███╗   ███╗     ██████╗ ██████╗  █████╗  ██████╗██╗  ██╗
██╔════╝██╔══██╗██╔══██╗████╗ ████║    ██╔════╝██╔═══██╗██╔══██╗██╔════╝██║  ██║
██║     ██████╔╝███████║██╔████╔██║    ██║     ██║   ██║███████║██║     ███████║
██║     ██╔══██╗██╔══██║██║╚██╔╝██║    ██║     ██║   ██║██╔══██║██║     ██╔══██║
╚██████╗██║  ██║██║  ██║██║ ╚═╝ ██║    ╚██████╗╚██████╔╝██║  ██║╚██████╗██║  ██║
 ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝     ╚═════╝ ╚═════╝ ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝
                    E X A M   P A N I C   M O D E   E D I T I O N
```

> `$ whoami` → the AI TA you call when it's 2am, the exam is in 6 hours,
> and your notes are a voice memo of you talking to yourself.

---

### `$ cat about.txt`

**CramCoach** takes a rambled voice note, a photo of your handwritten
notes, or an uploaded **PDF / DOCX / JPG** and converts it — via native
audio + vision + document understanding in **Gemini 2.0 Flash** — into
a triaged, exam-ready study guide, an editable flashcard deck, and a
difficulty-tagged quiz that's **auto-graded on the spot**. Everything is
shaped by how many hours you actually have left.

```
[ VOICE RAMBLE ]   --\
[ PHOTO OF NOTES ] ---\
[ PDF UPLOAD ]      ----->-- [ GEMINI (AUDIO / VISION / DOC) ] --> [ STUDY GUIDE + CARDS + QUIZ ] --> [ AUTO-GRADED SCORE ]
[ DOCX UPLOAD ]     --/       (DOCX text extracted first, rest sent natively)
[ JPG UPLOAD ]      -/
```

---

### `$ ls features/`

```
drwxr-xr-x  native_audio_input.py     # st.audio_input, no manual transcription step
drwxr-xr-x  native_vision_ocr.py      # st.camera_input, reads handwritten/printed notes directly
drwxr-xr-x  file_upload_pipeline.py   # st.file_uploader: PDF & JPG sent natively, DOCX text-extracted
drwxr-xr-x  quad_input_mode.py        # single pipeline handles audio, photo, PDF, or DOCX text
drwxr-xr-x  urgency_engine.py         # "hours until exam" drives triage + tone
drwxr-xr-x  difficulty_tiers.py       # Easy / Medium / Hard / Panic Mode
drwxr-xr-x  local_autograde.py        # quiz graded in Python, zero extra API cost
drwxr-xr-x  editable_flashcards.py    # st.data_editor deck, add/edit/delete rows
drwxr-xr-x  readiness_trend.py        # st.line_chart of score across cram sessions
```

---

### `$ ./setup.sh`

**1. Clone the repo**
```bash
git clone https://github.com/<your-username>/cram-coach.git
cd cram-coach
```

**2. Create a virtual environment**
```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Get a Gemini API key**
```
> https://aistudio.google.com/apikey
```
Paste it into the sidebar at runtime — never stored in the repo.

**5. Run locally**
```bash
streamlit run app.py
```

---

### `$ cat architecture.md`

Full system design + Mermaid diagram: [`ARCHITECTURE.md`](./ARCHITECTURE.md)
Full technical design doc: [`DESIGN_DOC.md`](./DESIGN_DOC.md)

---

### `$ curl -I deployment`

```
Status: 200 LIVE
Host:   Streamlit Community Cloud
URL:    https://<your-app-name>.streamlit.app
```
*(Replace with your actual deployment link before submission.)*

---

### `$ cat tech_stack.txt`

```
Frontend / App Framework .... Streamlit
AI Model .................... Gemini 2.0 Flash (native Audio + Vision + PDF Input, Structured Output)
Data Handling ................ Pandas
Document Parsing ............. python-docx (DOCX text extraction only)
Deployment ................... Streamlit Community Cloud
Version Control .............. Git + GitHub
```

---

### `$ echo $DISCLAIMER`

> Educational capstone project. AI-generated study material is a study
> aid, not a substitute for your actual course materials.

---

### `$ whoami --author`

Built by `<your name>` for the **MirAI School of Technology Capstone**
— Category B: EdTech & Campus Survival (`#8 Voice-Notes to Flashcards`,
customized into Exam Panic Mode).

`⭐ Star this repo if the Cram Coach got you through finals.`
