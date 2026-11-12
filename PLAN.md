# Local Student AI Workspace — Consolidated Plan & Research

**Repository**: https://github.com/XtraordinaryProxy/local-student-ai-workspace  
**Last Updated**: November 12, 2026  
**Status**: Research & Design Document (for sharing with other AI tools and collaborators)

---

## 1. Vision

A privacy-first, fully local (offline-capable) desktop AI workspace designed for students on budget hardware.

**Core goals**:
- Smart file & folder organization (intelligent naming, arranging, preview + undo)
- Offline RAG so students can ask questions about their own notes, PDFs, and materials without internet
- Usable on 8GB RAM laptops (low-budget students) and more capable on 16GB machines
- Support for diverse student needs: general/tech, software engineering, project management, finance, art, music, film, and vibe-coding beginners / AI specialists
- Hybrid approach: rules first, local AI only when it adds clear value
- Smart model management: download models while online, then run fully offline

Nothing leaves the machine. Everything works without an internet connection after the initial model downloads.

---

## 2. Target Users & Hardware Tiers

| Tier | RAM | Typical User | Recommended Models | Primary Use |
|------|-----|--------------|--------------------|-------------|
| **Low** | 8GB | Budget students | 3B–4B at Q4_K_M | File organizer + offline RAG + summaries + light study tools |
| **Standard** | 16GB | Serious students, vibe coders, AI specialists | 7B–14B at Q4 or Q5 | All of the above + stronger coding, CLI, structured output, tool-calling |

**Smart model switching**:
- Detect system RAM on first launch.
- Recommend and download appropriate models while online.
- Allow manual or task-based switching (Study mode vs Coding mode).
- Once downloaded, everything runs offline.

---

## 3. Core Features (All Brainstormed Ideas Preserved)

### 3.1 Smart File & Folder Organizer
- Intelligent renaming based on content (not just filename)
- Automatic grouping of related files (by assignment, project, topic)
- Duplicate detection with safe cleanup suggestions
- **Preview / dry-run mode** + full undo history (safety first)
- Hybrid: rule-based first, local AI for ambiguous cases

### 3.2 Offline RAG (Ask Your Files)
- Ingest PDFs, DOCX, notes, code, slides, etc.
- Local embeddings + lightweight vector store (Chroma embedded, FAISS + SQLite, or sqlite-vec)
- Structure-aware + recursive chunking with overlap and metadata
- Semantic search + chat over selected folders or the entire library
- Every answer includes source citations (file + approximate section)

### 3.3 Study & Productivity Tools
- One-click summaries
- Generate flashcards and practice questions from selected materials
- “Explain this simply” / “Explain like I’m in first year”
- Project / Assignment mode (drop files into a project folder → instant summaries + chat)

### 3.4 Domain-Specific Extensions
| Domain | Useful Capabilities |
|--------|---------------------|
| **Software Engineer / Vibe Coding** | Code explanation, docstring generation, local codebase search, structured output for scripts |
| **Project Manager / Professional** | Extract action items from notes, generate checklists/timelines from documents |
| **Finance** | Explain formulas, summarize case studies and local spreadsheets/PDFs |
| **Art** | Organize reference images and mood boards (with optional tiny vision model) |
| **Music** | Organize sheet music, practice notes, theory PDFs |
| **Film** | Organize scripts, shot lists, research; extract scenes/characters |
| **AI Specialist / Advanced** | Stronger models for coding, CLI-style work, API/tool-calling patterns |

### 3.5 Smart Model Download & Offline Mode
- First launch (online): detect RAM → recommend models → download via Ollama (or equivalent).
- Clear offline indicator once downloads complete.
- Mode switcher: Study / Coding / Organizer.
- Support for quantization levels (default Q4_K_M; optional Q5/Q6 when headroom exists).
- KV cache quantization (q8_0) for longer contexts on limited RAM.

---

## 4. Technical Architecture (High Level)

```
[Desktop App — Electron/Tauri or similar]
        ↓
[File Organizer Layer] — rules + local AI (preview + undo)
        ↓
[Document Processor] — extract text from PDF/DOCX/notes/code
        ↓
[Local Knowledge Layer]
  • Chunking (structure-aware + recursive + overlap)
  • Local embeddings (e.g. nomic-embed-text or similar)
  • Lightweight vector store
        ↓
[Query Engine] → retrieves relevant chunks → sends to local LLM
        ↓
[Local LLM Runtime] — Ollama (OpenAI-compatible API)
  • 8GB tier: 3B–4B Q4_K_M
  • 16GB tier: 7B–14B Q4/Q5 + optional stronger coding models
  • Optional tiny vision model for art/film image organization
```

**Key constraints respected**:
- Fully offline after model download
- Privacy-first (no data leaves the machine)
- Practical on 8GB RAM (leave 2–3GB for OS + system)
- Hybrid rules + AI for efficiency

---

## 5. Local Model & Quantization Guidance

**Default recommendation**: Q4_K_M (best quality-per-GB in 2026).

| Quant | Quality | Typical Use |
|-------|---------|-------------|
| Q4_K_M | Very good | Default for 8GB and most 16GB workloads |
| Q5_K_M / Q6_K | Excellent / near-lossless | 16GB machines with headroom (especially coding) |
| Q8_0 | Practically lossless | Only when memory is plentiful |

**Rules of thumb**:
- Prefer a larger model at Q4 over a smaller model at Q8.
- On 8GB: stay in the 3B–4B range.
- On 16GB: 7B–14B becomes practical for coding and structured work.
- Use KV cache quantization (q8_0) to free memory for longer study sessions.

Models are downloaded once via Ollama while online, then used fully offline.

---

## 6. Chunking Strategy (Offline RAG)

- **Structure-aware** when possible (respect headings, paragraphs, code blocks).
- **Recursive** fallback for unstructured text.
- **Overlap**: 10–20% to preserve context across chunk boundaries.
- **Metadata**: file path, page/section, heading hierarchy, document type.
- Optimized for small context windows of the local models.
- Incremental indexing so students can add new materials without full reprocessing.

---

## 7. Phased Roadmap

**Phase 1 (MVP)**  
Smart organizer + Ask Your Files (offline RAG) + basic summaries + preview/undo.

**Phase 2**  
Study tools (flashcards, practice questions), project mode, source citations, mode switching.

**Phase 3**  
Domain polish (code-aware features, image organization, specialized templates) + 16GB coding models.

---

## 8. Principles That Must Not Be Lost

- Privacy-first and fully local after download
- Usable on 8GB RAM laptops
- Safety features (preview, undo) for any file changes
- Hybrid rules + AI
- Source citations for every AI answer
- Support for both low-budget students and more advanced users (16GB, coding, vibe coding, AI specialists)
- Smart download-then-offline model management

---

## 9. Next Steps for Implementation

1. Choose desktop framework (Tauri preferred for lightness, or Electron).
2. Integrate Ollama for model management and inference.
3. Implement document ingestion + chunking + local vector store.
4. Build the organizer with dry-run + undo.
5. Add the chat/RAG interface with citations.
6. Add RAM detection and model recommendation/download flow.

---

## 10. Leveling Up the Student Workspace — High-Value Convenient Features

Research into current offline/local student tools (NotebookLM-style apps, Anki + FSRS, local RAG projects) shows the highest-value additions turn passive materials into **active learning** with almost zero extra effort.

### Highest-Impact Additions
1. **One-Click Active Learning Suite**  
   From any selected folder, PDF, or notes: generate flashcards (with optional local FSRS/SM-2 scheduling), quizzes/practice exams, one-page study guides, and ELI5 explanations.  
   *Value*: Active recall + spaced repetition delivers the biggest retention gains.  
   *Convenience*: Single button → reviewable materials ready in seconds. Optional Anki (.apkg) export.

2. **Mastery & Progress Tracking**  
   Simple levels (Recognize → Understand → Recall → Apply), due-today review queue, weak-area highlighting, and basic streaks. All data stays local.

3. **Frictionless Project / Course Mode**  
   Drop files into a course folder → automatic indexing → instant summary, key concepts, flashcards, quiz, and chat. Visual “what’s due / what’s weak” panel.

4. **Socratic / Tutor Mode**  
   Grounded answers that can switch to hint mode or ask questions first. Always cites exact file/section. Optional understanding checks.

5. **Quick Capture Enhancements**  
   Drag-and-drop or paste → smart folder + naming suggestions. Related materials suggestions. One-click “create study set from this selection.”

6. **Lightweight Focus Helpers**  
   Simple Pomodoro tied to a course, “review due cards” dashboard widget, daily study brief from local data.

**Design principles**: Zero-to-study in under a minute. Everything grounded with citations. Progressive (start with organizer + Ask Your Files, then generation tools, then spaced repetition). Hardware-aware (works on 8GB; stronger on 16GB).

**Recommended priority**:
1. One-click flashcards + quizzes + study guides
2. Source-cited chat + summaries
3. Local spaced-repetition review queue
4. Project/course mode with weak-area highlighting
5. Socratic tutor mode
6. Light progress dashboard

These turn the tool into a daily study companion that saves time *and* improves retention while remaining fully local.

---

## 11. Medical Professionals — High-Value Seamless Needs

Medical students, residents, and clinicians need tools that handle massive information volume, strict privacy, and unreliable hospital internet. Local-first design is especially valuable because sensitive data never leaves the device.

### Highest-Value Features for Medical Users
| Need | Feature | Why It Matters |
|------|---------|----------------|
| Study & board prep | One-click flashcards, quizzes, study guides from lecture PDFs/slides | Anki + spaced repetition is standard; students want cards from *their* lectures |
| Quick clarification | Grounded “Ask my materials” + Socratic tutor mode | Explain mechanisms, differentials, or wrong answers without leaving the app |
| Clinical reference (education) | Local RAG over user-added guidelines, textbooks, or notes | Point-of-care questions during rotations; offline after indexing |
| Organization | Smart organizer for lectures, cases, research PDFs | Content-based naming and grouping with preview/undo |
| Privacy-safe drafting | Summarize or structure local notes (never real patient PHI) | Avoid cloud HIPAA risks |
| Quick reference | Offline calculators (doses, lab values, scoring systems) + model explanations | Common bedside needs, all local |
| Progress | Mastery tracking per topic/rotation | Knowledge must stick for years |

**Design principles for medical use**:
- Strict privacy by design with clear UI warnings against uploading real patient PHI.
- Source-grounded answers only (citations from user files or local documents).
- Fully usable offline after model download.
- Fits 8GB–16GB laptops (3–4B models for study tools; stronger models on 16GB).
- Education-first focus with explicit disclaimers (not a replacement for clinical judgment).

**Realistic high-impact additions**:
- Medical study mode with optimized templates for Anki-style cards and board-style questions.
- Easy import and indexing of local guideline/textbook PDFs with strong citation UX.
- Local structured reference data (doses, lab ranges) that the model can explain.
- Case study organizer and weak-topic review queue from quiz performance.

---

## 12. HIPAA-Aligned Local AI Models

**Critical point**: No model is inherently “HIPAA-compliant.” Compliance depends on the full system (data handling, access controls, logging, encryption, transmission). Running models **locally** (on-device or on-premises) is one of the cleanest approaches because PHI never leaves the controlled environment, eliminating the need for a BAA with a cloud AI provider for the inference step.

Local inference still requires proper safeguards (access controls, audit logs, encryption, device security, risk assessment). Models can hallucinate; they are tools for study and support, not clinical replacements.

### Practical Local / On-Device Options (2026)
| Model | Size | Best For | Hardware Fit | Notes |
|-------|------|----------|--------------|-------|
| **MedGemma 4B** (Google) | ~4B | Medical text + images | 8–16 GB RAM | Official Ollama support; strong laptop balance |
| **MedGemma 27B** | 27B | Higher-quality clinical reasoning | 24 GB+ VRAM | Better accuracy; more resource-heavy |
| **MedPsy 1.7B / 4B** | 1.7–4B | Private medical Q&A | Phone to low-end laptop | Apache 2.0; fully on-device |
| **Qwen 2.5 / 3 series** | 7–14B | General + medical tasks, de-identification | 8–16 GB | Strong PHI detection in studies |
| **Llama 3.1 / 3.2** (8B) | 8B | General clinical text, note summarization | 8–16 GB | Widely used baseline |

**Most practical for student/resident laptop workspace**: MedGemma 4B (via Ollama) or strong general 3–7B models + offline RAG.

### Recommended Privacy Architecture
- Runtime: Ollama (or llama.cpp) fully offline.
- Data: Local vector store for RAG over personal notes/guidelines.
- Safeguards: UI warnings against real PHI, optional local de-identification, audit logs, full-disk encryption, device access controls.
- Usage focus: Personal study materials, de-identified cases, and local guideline PDFs.

This setup provides useful AI assistance for medical education while staying privacy-aligned with HIPAA principles and working in low-connectivity environments.

---

This document consolidates all research and ideas from the design sessions (including student feature leveling, medical professional needs, and HIPAA-aligned local models) and is intended to be shared with other AI tools, collaborators, or future implementation agents.

**End of Plan**
