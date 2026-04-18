[![License](https://img.shields.io/badge/License-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Concept%20under%20design-yellow)](https://github.com/enriqueherbertag-lgtm/LegacyModernizer)
[![AI Assistance](https://img.shields.io/badge/AI%20Assistance-DeepSeek-brightgreen)](https://deepseek.com)

# LegacyModernizer: COBOL Modernization Protocol for Banks

**A system that learns COBOL in real time, translates to modern languages (Python/Go), and validates in shadow mode, all within the bank's intranet.**

This project is not commercial software. It is a **working protocol** that combines:
- Small Language Models (SLMs) specifically trained for COBOL.
- A pipeline for extraction, translation, validation, and switchover.
- A security system based on single-use keys and a visitor timer.
- A hot modernization methodology (no mainframe shutdown).

**Philosophy:** The bank never stops. Code is translated in parallel. Risk is mitigated with shadow mode.

---

## ⚠️ The problem it solves

| Problem | Consequence |
|:---|:---|
| COBOL code is decades old, undocumented, ownerless. | No one really knows how it works. |
| Veteran programmers retire. | Knowledge is lost. |
| Migrating to modern languages is expensive and risky. | Banks postpone migration until it's too late. |
| Commercial tools translate poorly (spaghetti code). | The result is worse than the original. |

**Our solution:** A protocol that learns from COBOL while it runs, translates to clean code, and validates every transaction before switching over.

---

## 🧠 General architecture

[Mainframe COBOL] -> [Proxy/Sniffer] -> [Rule Extractor] -> [Translator to Python/Go] -> [Shadow Mode] -> [Gradual Switchover]

(Real-time Backup)   (Validated Rules)     (Generated Code)       (Migrated Module)

**All components run within the bank's intranet, with no internet access.**

---

## 📦 System components

### 1. Local AI models (no internet)

| Model | Function | Size | Training |
|:---|:---|:---|:---|
| `cobol-bert` | Extract business rules, dependencies, data flows. | 110M params | Fine-tuned from CodeBERT with labeled COBOL code. |
| `cobol2py` | Translate COBOL statements to Python/Go. | 350M params | Fine-tuned from CodeT5 with (COBOL, Python) pairs. |
| `code-llama-7b` (quantized) | Validate output equivalence in shadow mode. | 7B params (4-bit) | Quantized to run on CPU. |

**Models are trained outside the bank (with public data) and installed locally on their servers.**

### 2. Modernization pipeline (8 phases)

| Phase | Name | What it does | Automation |
|:---|:---|:---|:---|
| 0 | Preparation | Install models and environment on the bank's servers. | DevOps + You |
| 1 | Initial scan | Analyze all of the bank's COBOL code. | Automatic |
| 2 | Rule extraction | Generate a dependency graph and business rules. | Automatic |
| 3 | Human validation | Senior programmer reviews and corrects extracted rules. | Human (dashboard) |
| 4 | Automatic translation | Generate Python/Go code from validated rules. | Automatic |
| 5 | Shadow mode | Run new code in parallel, compare outputs. | Automatic |
| 6 | Human correction | Review discrepancies, correct code or model. | Human + Automatic |
| 7 | Switchover | Replace COBOL with new code (gradual). | Bank architect |
| 8 | Continuous maintenance | Repeat cycle for new modules or changes in COBOL. | Automatic + Human |

### 3. Shadow mode (continuous validation)

Real transaction -> [Original COBOL] -> COBOL_Output
                -> [Generated Code] -> Python_Output
                                  -> [Comparator] -> Match?
                                                     Yes: Log success
                                                     No: Log error + save inputs/outputs

**Shadow mode runs in parallel with the live system, without affecting production.**

### 4. Real-time backup

- Every transaction passing through the mainframe is copied to a modern database (PostgreSQL, Kafka, etc.).
- If the mainframe fails, data is not lost.
- The backup is **read-only** (does not write to the mainframe).

### 5. Security: Access protocol for external technicians

| Step | Action |
|:---|:---|
| 1 | Technician arrives at the bank. |
| 2 | Identifies themselves (ID, credentials, code). |
| 3 | Authorization is confirmed against the external personnel list. |
| 4 | Visitor timer is activated (start time). |
| 5 | A sealed envelope with admin keys is handed over. |
| 6 | Technician works (only during authorized hours). |
| 7 | Upon finishing, the timer deactivates and keys expire. |
| 8 | The next day, the process repeats with a new envelope. |

**The technician never has access outside their shift. The bank never loses control.**

---

## 🛠️ Technical implementation (summary)

### Bank requirements (infrastructure)

| Resource | Specification | Use |
|:---|:---|:---|
| Servers (2-4) | Modern CPUs (16+ cores), 64-128 GB RAM | Run models and pipeline |
| Storage | 1-2 TB SSD | COBOL code, models, logs |
| Network | Bank intranet | Access to code and test transactions |
| Optional (but recommended) | 1 GPU (NVIDIA T4 or similar) | Accelerate shadow mode |

### Installation (one-time)

Copy models to servers (no internet)
scp ./cobol-bert-local user@server:/bank/models/
scp ./cobol2py-local user@server:/bank/models/
scp ./codellama-7b.Q4_K_M.gguf user@server:/bank/models/

Install dependencies (no internet, using local packages)
pip install --no-index --find-links ./offline-packages transformers torch

Configure access to COBOL repository
./setup.sh --cobol-path /bank/cobol/legacy --output /bank/output

text

### Daily use (automatic)
Nightly scan (cron job)
0 2 * * * /bank/scripts/scanner.sh --incremental

Continuous shadow mode (service)
systemctl start shadow-mode.service

Human validation dashboard (internal access)
https://bank-intranet/legacy-modernizer/dashboard

