# Technical Design Document — CramCoach

## 1. Problem Statement

Last-minute exam revision is chaotic: students have scattered notes,
limited time, and no easy way to know which concepts are actually
high-yield versus which are safe to skip. Existing flashcard tools
require manual entry, which itself eats into study time. CramCoach lets
a student simply talk through what they remember and get back a
prioritized, testable study kit in seconds.

## 2. Goals & Non-Goals

**Goals**
- Convert a rambled voice note, a photo of handwritten/printed notes, or
  an uploaded PDF/DOCX/JPG directly into a triaged study guide,
  flashcards, and a quiz — using native audio, vision, and document
  understanding wherever Gemini supports it natively, with local text
  extraction only where required (DOCX).
- Make the AI's triage decisions time-aware: a 2-hour cram session
  should look meaningfully different from a 2-day one.
- Provide immediate, local, cost-free auto-grading with per-difficulty
  weak-spot detection.
- Track readiness trend across multiple cram rounds in one sitting.

**Non-Goals**
- Long-term spaced-repetition scheduling across days/weeks (explicitly
  out of scope for this capstone — session-scoped only).
- Multi-user accounts or persistent server-side storage.
- Grading via the LLM itself — grading is intentionally deterministic
  and handled in Python for speed, cost, and consistency.

## 3. Data Flow

1. **Input capture**: the student picks an input mode via `st.radio`:
   `st.audio_input` (raw WAV bytes from the mic), `st.camera_input` (raw
   JPEG bytes from the webcam), or `st.file_uploader` accepting
   `.pdf`, `.docx`, `.jpg`/`.jpeg`. For uploads, the file extension
   determines handling: PDFs and JPGs are passed through as raw bytes;
   DOCX files are routed through `extract_docx_text()` first.
2. **DOCX text extraction** (upload path only): `python-docx` opens the
   uploaded bytes as a `Document`, concatenates all non-empty paragraph
   text plus any table cell text, and truncates to `MAX_DOCX_CHARS` if
   the document is unusually long — this is the only modality that does
   not go to Gemini as a binary `Part`.
3. **Context assembly**: `hours_left`, the selected `difficulty` tier,
   and a modality-specific instruction line (e.g. "listen to...", "read
   the attached photo of...", "read the attached PDF...", or "read the
   following extracted text...") are interpolated into an f-string
   prompt that instructs the model how aggressively to triage and how
   hard to make the quiz.
4. **Inference**: for audio/photo/PDF, the binary `Part` and the text
   prompt are sent together to `gemini-2.0-flash`; for DOCX, only the
   text prompt (with extracted text embedded) is sent. Either way, a
   fixed `system_instruction` defines the "Cram Coach" persona and the
   exact output schema (shared across all four modalities), with
   `response_mime_type="application/json"` forcing structured output.
4. **Parsing**: the raw text response is stripped of stray markdown
   fences and parsed with `json.loads`; malformed responses are caught
   and surfaced as a retryable error.
5. **State update**: the parsed dict populates `st.session_state.last_result`
   along with a human-readable `source_label` (Voice / Photo / PDF / DOCX
   / Photo File); flashcards are loaded into an editable
   `st.session_state.flashcards_df`.
6. **Quiz + grading**: quiz questions render as a `st.form` of radio
   groups. On submission, a local Python loop compares each selected
   index against `correct_index`, computes a percentage score, and
   determines the weakest difficulty tier by frequency of misses —
   no additional API call is made for grading.
7. **History logging**: each graded session appends a row (topic,
   source, difficulty, score, timestamp) to `st.session_state.history`,
   which drives the `st.data_editor` table and `st.line_chart` readiness
   trend — letting the student compare, e.g., whether photo-based cram
   sessions score differently than PDF-based ones.

## 4. API Integration Strategy

- **Model**: `gemini-2.0-flash` — chosen for native multimodal audio,
  vision, and document understanding at low latency/cost, appropriate
  for a fast, iterative
  cram-session loop where a student may re-record several times.
- **Batching**: the audio capture and submit button are wrapped in a
  single `st.form`, guaranteeing exactly one Gemini call per explicit
  "Cram It" action rather than on every widget rerun.
- **Prompt engineering**: a persistent `system_instruction` defines the
  Cram Coach persona, tone constraints, and a strict JSON schema; the
  per-request prompt dynamically injects `hours_left` and difficulty via
  f-strings — this is what makes "Panic Mode" at 2 hours behave
  differently from "Easy" at 2 days, satisfying the rubric's system
  prompt + f-string + multimodality requirements together.
- **Temperature**: `0.8` keeps the coach's tone lively and varied while
  the forced JSON schema keeps output reliably parseable.

## 5. Logic Modules

| Module | Function |
|---|---|
| `init_state()` | Initializes history, last result, quiz answers/grading flags, and flashcard DataFrame in `st.session_state` |
| `generate_cram_kit()` | Builds the Gemini client, sends audio/image/PDF Part OR extracted DOCX text + dynamic prompt, returns parsed JSON (modality-agnostic across all four input types) |
| `extract_docx_text()` | Uses `python-docx` to pull paragraph and table text from an uploaded `.docx`, with a length guardrail (`MAX_DOCX_CHARS`) |
| Sidebar block | API key input, hours-left/difficulty controls, live readiness stats |
| Input mode selector | `st.radio` toggles between voice, photo, and file-upload capture widgets before the form |
| Capture form | `st.audio_input`, `st.camera_input`, or `st.file_uploader` (mode-dependent) + submit, single batched API trigger |
| File-type router | Inspects the uploaded file's extension to branch into PDF (native Part), JPG (native Part), or DOCX (text extraction) handling |
| Results section | Study guide expander, editable flashcard `st.data_editor` |
| Quiz section | Radio-based question form + local grading loop + per-question review expanders |
| History section | `st.data_editor` + `st.line_chart` of score trend across sessions (tagged by source), writes edits back into `st.session_state.history` |

## 6. Security & Privacy Considerations

- Gemini API key collected via `type="password"`, held only in-memory
  for the session, never written to disk or committed to the repo.
- No audio recordings, photos, or uploaded files are persisted to disk
  or any external store beyond the single Gemini inference call (or, for
  DOCX, the in-memory text extraction step).
- `.gitignore` should exclude any local `.env` file if used for testing.

## 7. Known Limitations

- Audio quality/background noise can affect how well Gemini interprets
  the ramble; no client-side audio quality validation in v1.
- A single input (voice note, photo, or file) captures one topic per
  cram round; multi-topic sessions require multiple submissions.
- DOCX files that are scanned images pasted into Word (no real text
  layer) will extract empty text — the app detects this and warns the
  user rather than sending an empty prompt.
- Very large PDFs may approach Gemini's inline-file size limits; no
  chunking/pagination strategy is implemented in v1.
- No authentication — appropriate for a single-user demo/capstone
  context, not production multi-tenant use.

## 8. Future Enhancements

- Multi-day spaced-repetition scheduling using the flashcard deck.
- Export flashcards to Anki-compatible `.apkg` format.
- Persistent storage (SQLite/Firebase) so readiness trends survive
  across browser sessions, not just within one.
- Optional follow-up Q&A chat grounded in the uploaded material.
- Support for multi-file uploads (e.g., a full slide deck across
  several PDFs) in a single cram session.
