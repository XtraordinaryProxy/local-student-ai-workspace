# Local Student AI Workspace — Consolidated Plan & Research

**Repository**: https://github.com/XtraordinaryProxy/local-student-ai-workspace  
**Last Updated**: November 2026  
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

This document consolidates all research and ideas from the design sessions and is intended to be shared with other AI tools, collaborators, or future implementation agents.

**End of Plan**
