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

### Sprint 0 · Kick‑off ( May 25 – May 31 )
- [ ] Create private Git repo, MIT license, `.gitignore`.
- [ ] Add this README & `PROJECT_PLAN.md`.
- [ ] Set up conda/venv, install PyTorch, transformers, miditok.
- [ ] Verify GPU with `torch.cuda.is_available`.
- [ ] *(Optional)* Sketch future backlog in GitHub issues.

### Sprint 1 · Data Bootstrap ( Jun 1 – Jun 14 )
- [ ] **Download XMIDI** archive & verify checksum.
- [ ] Parse filenames → genre labels; unit‑test 50 samples.
- [ ] **Phrase extractor**: cut MIDI into 2–4‑bar clips.
- [ ] Save clips to `data/processed/phrases`.
- [ ] **EDA notebook**: genre counts, ≥ 6 k classical & jazz each.
- [ ] Play 3 random clips/genre to ear‑check.
- [ ] *(Opt)* Curate ≥50 funk MIDIs; log counts.
- [ ] *(Opt)* Extra EDA: tempo & key histograms.

### Sprint 2 · Tokenization Pipeline ( Jun 15 – Jun 28 )
- [ ] Install & configure **MidiTok Octuple** vocab.
- [ ] Implement `phrase_to_tokens()`; test on 10 clips.
- [ ] Tokenise **1 k phrases**; cache HF Arrow dataset.
- [ ] Dataset wrapper with padding & attention mask.
- [ ] Decode‑back sanity check (tokens → MIDI) on 2 clips.
- [ ] Log avg sequence length & vocab size.
- [ ] *(Opt)* Tokenise 100 phrases with REMI for later compare.
- [ ] *(Opt)* Include Program tokens for instrumentation.

### Sprint 3 · Baseline Model ( Jun 29 – Jul 12 )
- [ ] Define `MusicConfig` (vocab, 512 max_len, 6 labels).
- [ ] Build 6‑layer Transformer (256 dim, 8 heads).
- [ ] Trainer loop (AdamW, CE loss, accuracy metric).
- [ ] Train on 1 k sample set (≤30 min) → `baseline.ckpt`.
- [ ] Save val confusion matrix & accuracy ≥ 60 %.
- [ ] Document baseline numbers.
- [ ] *(Opt)* Binary classifier classical vs jazz quick check.
- [ ] *(Opt)* Logistic‑reg baseline on hand‑crafted stats.

### Sprint 4 · Genre‑Focused Tuning ( Jul 13 – Jul 26 )
- [ ] Tokenise **full dataset** (multiprocess).
- [ ] Stratified split by song (train/val/test).
- [ ] Apply class‑weights / oversample classical & jazz.
- [ ] Enable fp16, tune LR & dropout; train full model.
- [ ] Achieve **F1_classical ≥ 0.80, F1_jazz ≥ 0.75**.
- [ ] Log per‑class metrics & plots.
- [ ] *(Opt)* Transpose‑aug jazz phrases ±2 semitones.
- [ ] *(Opt)* t‑SNE on CLS embeddings to visualise clusters.

### Sprint 5 · Expand Other Genres ( Jul 27 – Aug 9 )
- [ ] Add curated funk/folk MIDIs; label & tokenise.
- [ ] Apply transposition / velocity augment for scarce genres.
- [ ] Fine‑tune model 3 epochs on expanded data.
- [ ] Validate: non‑primary genres ≥ 65 % acc; classical/jazz unchanged.
- [ ] Record final dataset stats & confusion matrix.
- [ ] *(Opt)* Tempo‑swing jitter augmentation for jazz.
- [ ] *(Opt)* W&B LR sweep for last‑mile tuning.

### Sprint 6 · Comparative Experiments ( Aug 10 – Aug 23 )
- [ ] Train tiny model on **REMI** subset; log acc & speed.
- [ ] Train 4‑layer vs 8‑layer model on Octuple subset.
- [ ] Compile comparison table & charts.
- [ ] Write **mini‑report** (Markdown).
- [ ] *(Opt)* Jazz phrase generation PoC – train decoder, sample 4‑bar MIDI.
- [ ] *(Opt)* Linear‑probe emotion labels on CLS embeddings.

### Sprint 7 · Demo & Wrap‑Up ( Aug 24 – Sep 6 )
- [ ] Export final model & tokenizer; upload to repo or HF Hub.
- [ ] Build **Gradio** or notebook demo: upload MIDI → genre probs.
- [ ] Package `predict.py` CLI and usage docs.
- [ ] Write final README/report with KPIs & lessons.
- [ ] Clean code, tag release `v1.0`.
- [ ] Record GIF or 60‑s screen capture of demo.
- [ ] *(Opt)* Embed audio in demo; blog post or slide deck.

---

## 5 · Progress Tracking
Tick checkboxes as tasks complete and append a sprint retro.

### Retro template
```
### Retro – Sprint N (ended YYYY‑MM‑DD)
✅ finished: …  ❌ slipped: …
Hours spent: …
Classical F1: …  Jazz F1: …
Notes…
```

---

## 6 · Useful Commands

```bash
# activate env
conda activate midi‑genre

# tokenise 1 k sample
python tokenize.py --input data/processed/phrases --limit 1000

# train baseline
python train.py --config configs/baseline.yaml

# run prediction
python predict.py --midi example.mid
```

---

## 7 · Future Roadmap
1. Self‑supervised pre‑training on full XMIDI.  
2. Larger MusicBERT‑style model for phrase embeddings.  
3. Jazz improvisation generator.  
4. Real‑time genre detection plugin (VST).

---

*Deliver value in small, verifiable increments; keep classical & jazz accuracy first.*  
Happy hacking 🎶
