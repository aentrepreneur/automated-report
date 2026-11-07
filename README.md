<!-- Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
<div align="center">

# Automated Report

Template-driven document & reporting pipeline — SOC reports, executive summaries, MOP procedures, PowerPoint presentations

![Status](https://img.shields.io/badge/v1.0.0-28a745?style=flat-square)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.12+-blue?style=flat-square&logo=python)](https://python.org)
![Tests](https://img.shields.io/badge/tests-126%20passing-brightgreen?style=flat-square)
![Updated](https://img.shields.io/github/last-commit/aentrepreneur/automated-report?style=flat-square)

</div>

> [!NOTE]
> Automated Report fusiona dos proyectos previos: **SOC Reporting v2.0.0** (SOC reporting pipeline) y **auto-docs v2.0.0** (template-driven document generator). Ambos comparten el mismo patrón arquitectónico pipeline source→process→render→deliver.

## Overview

Automated Report convierte datos estructurados en documentos profesionales listos para entrega, con gráficos embebidos y envío automatizado.

### Perfiles de Generación

| Perfil | Input | Output | Uso |
|--------|-------|--------|-----|
| **SOC Reports** | SQL queries | .docx + charts → email (SES) | Reportes periódicos de seguridad |
| **Doc Generation** | YAML/JSON/TXT | .docx (MOP), .docx (Ejecutivo), .pptx | Procedimientos, reportes ejecutivos, presentaciones |

## Architecture

```
Source ──► Process ──► Render ──► Deliver
  │             │           │           │
  ├─ SQL        ├─ pandas   ├─ python-   ├─ Amazon SES
  ├─ YAML       ├─ DataFrames│ docx      ├─ IMAP fetch
  ├─ JSON       ├─ chart     ├─ python-   ├─ File output
  └─ TXT       │  data      │  pptx     └─ Batch queue
               │            │
               └─ pipeline  └─ 6 chart    (YAML job list)
                  retry        types:
                  (exponential  bar, pie,
                   backoff)    scatter,
                               histogram,
                               line, multi
```

## Features

| Feature | SOC Reports | Doc Generation |
|---------|-------------|----------------|
| SQL query → document | ✅ | — |
| YAML/JSON/TXT input | — | ✅ |
| Bar, pie, scatter, hist, line, multi_line charts | ✅ | ✅ |
| Email delivery (Amazon SES) | ✅ | — |
| IMAP attachment processing | ✅ | — |
| MOP / Executive / PPTX output | — | ✅ |
| Batch mode (YAML job list) | — | ✅ |
| Template-driven (builder pattern) | — | ✅ |
| OPC/Dublin Core metadata | — | ✅ |
| Exponential backoff retry | ✅ | — |
| Standalone .exe builds (PyInstaller) | ✅ | — |

## Quick Start

```bash
# SOC Report: SQL → docx → SES
python -m automated_report --source sql --query "SELECT * FROM incidents" --output report.docx

# Document Generation: YAML → MOP
python -m automated_report --source yaml --template mop --config procedure.yaml --output procedure.docx

# Document Generation: YAML → PPTX
python -m automated_report --source yaml --template presentation --config brief.yaml --output brief.pptx

# Batch mode
python -m automated_report --batch jobs.yaml
```

## Documentation

- `docs/ARCHITECTURE.md` — pipeline design and retry logic
- `docs/DOC-GEN-PROFILES.md` — document generation templates and builder patterns

## Examples

- `examples/soc-report-sample.md` — SOC reporting workflow
- `examples/doc-gen-sample.md` — document generation workflow

## Ecosystem

Part of the aentrepreneur ecosystem. See [DevSecOps Agent](https://github.com/aentrepreneur/devsecops-agent) for complementary vulnerability lifecycle automation.

## License

MIT — see [LICENSE](LICENSE)

## Author

Angel Esquivel — [@aentrepreneur](https://github.com/aentrepreneur)

<!-- End Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
