[![Licencia](https://img.shields.io/badge/Licencia-Copyright%20%28c%29%202026%20Enrique%20Aguayo-red)](LICENSE)
[![Estado](https://img.shields.io/badge/Estado-Concepto%20en%20diseño-yellow)](https://github.com/enriqueherbertag-lgtm/LegacyModernizer)
[![Asistencia IA](https://img.shields.io/badge/Asistencia%20IA-DeepSeek-brightgreen)](https://deepseek.com)
[![Seguridad](https://img.shields.io/badge/Seguridad-Acceso%20y%20Claves-blue)](./docs/seguridad.md)
[![Shadow Mode](https://img.shields.io/badge/Validación-Shadow%20Mode-blue)](./docs/shadow_mode.md)
[![Cláusula de Control Humano](https://img.shields.io/badge/Control%20Humano-No%20Caja%20Negra-red)](./docs/LEGACYMODERNIZER.md)

# LegacyModernizer: Secure COBOL Modernization Protocol for Banks

**A low-risk protocol to modernize legacy COBOL systems into modern languages (Python/Go), using local AI, business rule extraction, and shadow mode validation — all running inside the bank's intranet.**

This is not a ready-to-use commercial product. It is a **working protocol** designed for banks that need to migrate their critical systems without stopping operations or taking unacceptable risks.

**Philosophy:** The bank never stops. Modernization happens in parallel. Risk is controlled with shadow mode and human supervision.

---

## The Problem It Solves

| Problem | Consequence |
|---------|-------------|
| Over 220 billion lines of COBOL are still in production worldwide | Critical systems depend on old and poorly documented code |
| The average COBOL programmer is ~58 years old and ~10% retire each year | Critical institutional knowledge is being lost |
| "Big bang" migrations fail in 70-80% of cases | Million-dollar costs and major service disruptions |
| Traditional automated tools often produce hard-to-maintain code | The final result is frequently worse than the original system |

**Our Solution:** An incremental protocol that combines local AI analysis, business rule extraction, assisted translation, and shadow mode validation, enabling a safe and gradual switch.

---

## High-Level Architecture

Mainframe COBOL → Proxy/Sniffer → Rule Extractor → Assisted Translator (Python/Go) → Shadow Mode → Gradual Switch


**All processes run exclusively inside the bank's intranet**, with no internet connection.

---

## Main Components

### 1. Local AI Models (Offline)

| Model                        | Main Function                                | Approximate Size     | Notes |
|------------------------------|----------------------------------------------|----------------------|-------|
| `cobol-analyzer`             | Business rule and dependency extraction      | 110-350M parameters  | Fine-tuned on COBOL code |
| `cobol2modern`               | Assisted translation to Python/Go            | 350M-7B parameters   | Quantized for local CPU/GPU |
| Equivalence Validator        | Output comparison in shadow mode             | Local LLMs           | Focus on accuracy |

Models are prepared outside the bank and installed locally.

### 2. Modernization Pipeline

| Phase | Name                      | Description                                              | Responsible |
|-------|---------------------------|----------------------------------------------------------|-------------|
| 0     | Preparation               | Install models and secure environment                    | DevOps + Bank Team |
| 1     | Scan & Inventory          | Analyze all COBOL codebase                               | Automatic |
| 2     | Rule Extraction           | Generate dependency graph and business rules             | Automatic + AI |
| 3     | Human Validation          | Review by veteran programmers                            | Human (dashboard) |
| 4     | Assisted Translation      | Generate clean modern code                               | Automatic + AI |
| 5     | Shadow Mode               | Parallel execution and output comparison                 | Automatic |
| 6     | Correction & Iteration    | Fix discrepancies                                        | Human + AI |
| 7     | Gradual Switch            | Module-by-module replacement                             | Bank Architect |
| 8     | Continuous Maintenance    | Monitor and update migrated modules                      | Automatic + Human |

### 3. Shadow Mode (Parallel Validation)

Real transaction → Original COBOL → COBOL Output  
→ Modern Code → Python/Go Output  
→ Comparator → Do the outputs match?

- Match → Log success  
- No match → Log error + save input/output for review

**Shadow mode does not affect production.**

### 4. Real-time Backup
- Critical transactions are copied to a modern database (PostgreSQL + Kafka).
- Protects data in case of mainframe failure.
- Read-only backup.

### 5. Security Protocol for External Technicians

| Step | Action |
|------|--------|
| 1    | Technician arrives and identifies |
| 2    | Authorization is verified |
| 3    | Visitor timer is activated |
| 4    | Sealed envelope with daily admin key is delivered |
| 5    | Work is performed only during authorized hours |
| 6    | Timer expires and key becomes invalid at end of day |

---

## Recommended Infrastructure

| Resource         | Specification                              | Purpose |
|------------------|--------------------------------------------|---------|
| Servers          | 2-4 servers (16+ cores, 64-128 GB RAM)     | Models and pipeline |
| Storage          | 1-2 TB SSD NVMe                            | Code, models and logs |
| Network          | Bank intranet (isolated)                   | All operations |
| GPU (optional)   | 1× NVIDIA T4 or equivalent                 | Accelerate validation |

---

## License

Copyright © 2026 Enrique Aguayo. All rights reserved.

**Allowed:** Non-commercial use for educational, research, or internal pilot purposes.

**Prohibited without express written permission:** Commercial use, production deployment, or distribution of modified versions.

For commercial licenses, technical support, or bank pilots:  
**Contact:** eaguayo@migst.cl

---

## Author

**Enrique Aguayo H.**  
Mackiber Labs  
Contact: eaguayo@migst.cl

*Documentation assisted by DeepSeek (AI).*
