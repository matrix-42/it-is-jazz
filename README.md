# Symbolic‑Music Genre Classifier – Project README  
*Solo, part‑time · May 25 – Sep 6 2025 · 2‑week sprints*  

## 1 · Purpose
Build a **MIDI phrase‑level** classifier that recognises  
**classical** & **jazz** (primary KPI) plus *pop, rock, funk, folk* (secondary).  
Results power future work on jazz generation.

## 2 · Key Design Choices
| Area | Final Decision | Rationale |
|------|----------------|-----------|
| **Dataset** | **XMIDI** (108 k MIDI, labelled) | Largest labelled symbolic set; ample classical & jazz.  |
| **Missing genres** | Curate small funk/folk set if needed | To avoid skew; low effort. |
| **Tokenizer** | **Octuple** (via MidiTok) | Compact (<60 tokens/4‑bar phrase), multi‑track, proven in MusicBERT. |
| **Model** | 6‑layer Transformer encoder (DistilBERT‑size) trained **from scratch** | High accuracy with moderate compute; transparent; focus resources on jazz/classical. |
| **Optional Experiments** | REMI / CP tokens; smaller & larger models; jazz phrase generation | For comparison report & next project boot‑strap. |

---

## 3 · Roadmap (7 × 2‑week Sprints)

| Sprint | Dates | Demo / Acceptance Criteria |
|--------|-------|----------------------------|
| **0** Kick‑off | **May 25 – May 31** | Repo skeleton, conda env, GPU check. |
| **1** Data Bootstrap | **Jun 1 – Jun 14** | XMIDI downloaded; ≥6 k classical & ≥6 k jazz phrases extracted; genre bar‑chart; sample playback. |
| **2** Tokenization Pipeline | **Jun 15 – Jun 28** | `tokenize.py` → Octuple IDs; 1k phrases tokenised; avg len < 60. |
| **3** Baseline Model | **Jun 29 – Jul 12** | 6‑layer encoder trains on 1 k set; val acc ≥ 60 %; confusion matrix saved. |
| **4** Genre‑Focused Tuning | **Jul 13 – Jul 26** | Full data fine‑tuned; **F1_classical ≥ 0.80, F1_jazz ≥ 0.75**. |
| **5** Expand Other Genres | **Jul 27 – Aug 9** | New funk/folk data integrated; all‑genre model with secondary classes ≥ 65 % acc. |
| **6** Comparative Experiments | **Aug 10 – Aug 23** | Mini‑report: Octuple vs REMI, model‑size A/B; (opt) jazz phrase generation sample MIDI. |
| **7** Demo & Wrap‑Up | **Aug 24 – Sep 6** | Gradio/Notebook demo; final model & tokenizer published; README/report complete. |

> **Time budget:** 5–8 h / week (≈10–14 h per sprint).  
> **Weekdays:** light tasks; **weekends:** heavy coding/training.

---

## 4 · Action Checklists
> The live checklist is tracked in **[Issue #1](https://github.com/matrix-42/it-is-jazz/issues/1)**.

---

## 5 · Future Roadmap
1. Self‑supervised pre‑training on full XMIDI.  
2. Larger MusicBERT‑style model for phrase embeddings.  
3. Jazz improvisation generator.  
4. Real‑time genre detection plugin (VST).

---

*Deliver value in small, verifiable increments; keep classical & jazz accuracy first.*  
Happy hacking 🎶
