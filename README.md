VERIQ - ALL BRANCH READMES (Team The Outliers, RAKSHAM Grand Finale, IIT Delhi)

WHAT EACH BRANCH IS FOR
- main: the landing page. Explains the project and points judges to the right branch.
- claude-c: THE SUBMISSION. The full working app (chat check, name matcher, rules, registry, reports, Kit runner), tests, benchmarks, results and all documents. Run the demo from here.
- claude-b-voice-results: the voice-note evaluation (ASVspoof run, own-voice Hinglish test, speech-to-text module). Kept separate, not merged; shown as 'to be reviewed' in claude-c.
- hansi-kaa-patch-1: small README edits only.


==================================================================
BRANCH: main  (save as README.md on this branch)
==================================================================

# VERIQ: check who you are paying, before you pay

**Team The Outliers · RAKSHAM Grand Finale, IIT Delhi (24–25 Oct 2026)**

Investment and "SEBI advisor" scams in India get victims to pay a UPI ID or bank account that belongs to someone
else. The chat says *"Navyug Wealth Research"*; the money goes to *"VIKRAM MOBILE POINT"*. VERIQ reads the chat
before you pay, lists every place you were asked to pay, compares the name **your own bank app** shows with the firm
the chat claims, and gives one answer per payee: why it fired, and what to do next.

It runs on your laptop, blocks nothing, contacts no one (no bank, UPI, SEBI or police system), and saves nothing.
It can be fooled, and we say how (`EVASION_LAB.md` on `claude-c`).

## Where the code is

| Branch | What it holds | Start here |
|---|---|---|
| **`claude-c`** | **The submission:** the full app, rules engine, name matcher, registry, reports, Kit runner, tests, benchmarks, results and all documents | [`README.md`](../../tree/claude-c) → `SUBMISSION.md` |
| `claude-b-voice-results` | The voice-evaluation work: ASVspoof 2019 LA run, length sweep, own-voice Hinglish test, speech-to-text module | `HANDOFF_B_FINAL.md`, `results/asvspoof/` |
| `main` | This landing page | |
| `hansi-kaa-patch-1` | README edits only | |

## Run it (Windows, from the `veriq` folder of `claude-c`)
```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1; pip install -r requirements.txt
powershell -ExecutionPolicy Bypass -File app\run_app.ps1 -SkipTests      # opens http://localhost:8501
```
Test any folder of chats / voice notes / QR images: `python -m eval --input <folder> --out preds.csv --report report.md`
(or the app's **Kit runner**). Reproduce every number: `reproduce.ps1` (see `REPRODUCE.md`).

## What we measured (every number has its command in `RESULTS.md` / `CLAIMS.md` on `claude-c`)

| Test | Result | Data |
|---|---|---|
| AI-generated blind chat set, 80 chats, metrics pre-registered (`bench/PREREG.md`) | **Before fixes (blind):** 28/40 scams warned, 7/40 genuine chats wrongly accused. **After fixes (not blind):** 35/40 and 1/40 | AI-generated, author model not recorded |
| Name pairs, 200 (AI-written, not blind) | a different firm called a "match": 4/100 → 1/100 after fixes | AI-generated, optimistic |
| Voice check, ASVspoof 2019 LA held-out | AUC 0.77; a **weak review cue only**, never clears a payee | public |
| Own-voice Hinglish test (branch `claude-b-voice-results`) | the voice check **fails** on Hinglish (AUC 0.085, n=60) | consented teammate clips + TTS |
| Code tests | 1,100+ pass; test plan maps every claim to its test | |

## Honest limits
A mule account opened in the firm's exact name passes the name check · pressure written without keywords is missed ·
voice can be fooled · the SEBI list is a synthetic dated snapshot (never a live lookup, by design) · speech-to-text is
built on `claude-b-voice-results` but its real-model run is not yet verified · no bank pilot is agreed.

## Data and AI use
All chats are synthetic or AI-generated and fictional; their generation code is in the repo. Voice data is public
(ASVspoof 2019 LA, Free Spoken Digit Dataset) or synthetic (Kokoro TTS). Claude (Anthropic) was used to write code and
documents under the team's direction; `DISCLOSURE.md` lists every model, dataset and library.

==================================================================
BRANCH: claude-c  (save as README.md on this branch)
==================================================================

# VERIQ: check who you are paying, before you pay

Team The Outliers · RAKSHAM Grand Finale, IIT Delhi (24-25 Oct 2026). VERIQ reads a suspicious money chat
(WhatsApp export, voice notes, QR screenshots), lists every place it asks you to pay, compares the name YOUR bank
app shows with the firm the chat claims, checks the quoted SEBI number against a dated list, looks for pressure
tactics and other victims' reports, and gives one answer per payee with its evidence and your next step. Voice is
only a review cue. It blocks nothing and contacts no one. It can be fooled (EVASION_LAB.md).

**Run:** Windows `powershell -ExecutionPolicy Bypass -File app\run_app.ps1` · macOS/Linux `bash app/run_app.sh`
**Check an install:** `python scripts/smoke.py` · **Reproduce every number:** `reproduce.ps1` or `make reproduce`
**Judges' own files:** `veriq-eval --input <folder> --out preds.csv --report report.md` (or the app's Kit tab)

## Start here (2 minutes)
1. Run the app (above) and open http://localhost:8501. Try **Try a synthetic demo** → A, or paste any chat.
2. Judges' test kit: left menu **For judges and reviewers → Kit runner**, upload a .zip (example: `demo/veriq_demo_kit.zip`,
   rebuilt by `python scripts/make_demo_kit.py`; a walkthrough kit we wrote, not a test set).
3. Results: toggle **How accurate is VERIQ?** in the same menu, or read `RESULTS.md`.

## Headline results (details and commands in RESULTS.md, results/prereg/)
| Test | Before fixes | After fixes (not blind) |
|---|---|---|
| AI-generated blind chats (n=80): scams warned / genuine accused | 28/40 / 7/40 | 35/40 / 1/40 |
| AI name pairs (n=200): different firm called a match | 4/100 | 1/100 |
| AI extraction lines (n=100): UPI + account IDs found | 18/53 | 50/53 |

## Branches
`claude-c` (this branch) is the submission. Voice evaluation (ASVspoof run, own-voice Hinglish test, speech-to-text)
is on `claude-b-voice-results` and is not merged here; the Plan vs delivered page marks those items "to be reviewed".

## Repository map
| Folder | What |
|---|---|
| `app/` | Streamlit app (6 steps, results page, Kit runner, scenario player, privacy proof) |
| `parsing/`, `extract/` | WhatsApp parser; payee, IFSC, link, SEBI-number and pressure-cue extraction (incl. hidden UPI IDs) |
| `match/`, `rules/`, `core/` | name matcher, decision ladder and SEBI check, shared contract (schemas) |
| `registry/`, `report/` | cross-victim registry (keyed hashes), 1930 filing sheet and redacted summary |
| `voice/`, `eval/` | voice pipeline (AASIST, weak cue) and `veriq-eval` / Kit runner |
| `bench/`, `generation/`, `data/` | benchmarks, pre-registration, synthetic and AI-generated datasets with their generation code |
| `tests/`, `scripts/`, `results/` | tests, reproduce/acceptance/demo scripts, every committed result |

| Read | For |
|---|---|
| SUBMISSION.md | the index: what, how to run, reproduce, data and licences, limits |
| RUBRIC_MAP.md | every rubric row → evidence, command, demo step |
| DEMO_12MIN.md, QA_SHEET.md | the live slot and the answers |
| RESULTS.md, CLAIMS.md, TESTPLAN.md | every number with its command; every claim with its test |
| PROBLEM_USER.md, DEPLOYABILITY.md, DEPLOY.md | user, scenarios, sources; who runs it, cost estimates, pilot needs |
| ENGINEERING_WALKTHROUGH.md, PLAN_VS_DELIVERED.md | what we built in the finale vs the Round 1 plan |
| PREBUILT.md, FINALE_LOG.md, DISCLOSURE.md | prototype boundary (tag `v0-prebuilt`), phase log, every model/dataset/library and AI use |
| docs/walkthrough/ | one plain-words card per module |

Claude (Anthropic) was used to write code and documents under the team's direction. All chat data is synthetic or
written by the team; voice data is public (ASVspoof 2019 LA, FSDD) or synthetic (Kokoro TTS).

==================================================================
BRANCH: claude-b-voice-results  (save as README.md on this branch)
==================================================================

# VERIQ voice evaluation (branch `claude-b-voice-results`)

This branch holds the **voice-note check evaluation** for VERIQ (Team The Outliers, RAKSHAM Grand Finale, IIT Delhi).
It is **not merged** into the submission branch `claude-c` and is shown there as "to be reviewed". The app, rules
engine and all other documents live on `claude-c`.

## What is here
| Item | Files |
|---|---|
| Voice pipeline: ffmpeg → quality gate → VAD → 64,600-sample chunks → pretrained AASIST (MIT) | `voice/` |
| Held-out ASVspoof 2019 LA run (dev threshold fixed on dev, scored once on eval) | `scripts/b_run_all.py`, `scripts/make_asvspoof_subset.py`, `results/asvspoof/` |
| Clip-length sweep, 0.1 s to 6 s | `scripts/length_sweep.py`, `results/asvspoof/length_sweep/` |
| Own-voice Hinglish set: 12 invented sentences, teammates record (with consent), Kokoro TTS makes the fakes | `scripts/hinglish_build.py`, `scripts/record_hinglish.py`, `data/hinglish/` |
| Speech-to-text for voice notes (faster-whisper, on the laptop) | `voice/transcribe.py` |
| One-command runners | `scripts/b_run_all.py`, `scripts/b_finish_all.py` |
| Tests | `tests/voice_eval/`, `voice/tests/` |

## Results (from `results/asvspoof/`)
| Test | Result | Data |
|---|---|---|
| ASVspoof 2019 LA, held-out eval | AUC **0.766**, EER 0.317 (n=600); at the dev threshold 15.7% of genuine clips flagged, 56.3% of synthetic clips missed | public (ODC-By); AASIST was trained on LA, so this is in-distribution |
| Own-voice Hinglish set (n=60: 12 real, 48 TTS) | **Fails:** AUC 0.085; 58% of real voices flagged, 98% of fakes missed | consented teammate clips (not in git, for privacy) + Kokoro TTS generated by us |
| Speech-to-text sample on the laptop | **0/6 transcribed** (TypeError in the first real-model run) | `results/asvspoof/B_STT_SAMPLE.json` |

Conclusion: the voice check is a **weak review cue only**. In VERIQ it can ask for human review and can never clear a
payee. It does not work on Hinglish voices today.

## Reproduce
```powershell
pip install -r voice/requirements.txt          # numpy + faster-whisper (not pinned yet)
pip install torch --index-url https://download.pytorch.org/whl/cpu
python scripts/b_run_all.py --zip LA.zip        # ASVspoof 2019 LA (download from the official source; seeds 1234 dev / 4321 eval)
python scripts/b_finish_all.py                  # length sweep, speech-to-text sample, Hinglish set, In-the-Wild probe (downloads nothing)
python scripts/hinglish_build.py prompts        # then teammates record: python scripts/record_hinglish.py --name <you> --device "<mic>"
python scripts/hinglish_build.py fakes --kokoro kokoro-v1.0.onnx --voices voices-v1.0.bin
python -m pytest tests/voice_eval voice/tests
```
Needs the AASIST weights (`git clone https://github.com/clovaai/aasist`, MIT) and ffmpeg on PATH. Full notes:
`HANDOFF_B_FINAL.md`. Claude (Anthropic) was used to write this code under the team's direction.
