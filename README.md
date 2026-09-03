<div align="center">

# SOCAR Reports

**Automated SOC reporting engine** — generación, personalización y envío de informes SOC.

[![Version](https://img.shields.io/badge/version-2.0.0-blue)]()
[![Python](https://img.shields.io/badge/python-3.12%2B-green)]()
[![License](https://img.shields.io/badge/license-Proprietary-red)]()

---

</div>

## Features

| Icon | Feature | Description |
|------|---------|-------------|
| 📊 | Report Generator | Genera informes .docx a partir de consultas SQL |
| 📧 | Email Sender | Envío automático con Amazon SES |
| 📎 | Attachment Processor | Descarga y procesa adjuntos IMAP |
| 📈 | Graphs | Gráficos automatizados (bar, pie, scatter, hist) |
| 🔄 | Retry Logic | Reintentos con backoff para errores transitorios |
| 🐍 | Python 3.12 | Type hints, zoneinfo, match/case |

## Architecture

```
source/
├── settings.py              # Pydantic config + RotatingFileHandler
├── database.py              # Conexión y queries DB (pyodbc)
├── utils.py                 # Helpers de fechas y texto
├── sql_processor.py         # SQL-like sobre DataFrames (pandas)
├── report_generator.py      # Orquestación de informes
├── set_socar_reports_v2.py  # Facade (re-exporta todo)
├── set_paragraphs.py        # Estilos y párrafos .docx
├── set_socar_graphs.py      # Generación de gráficos
├── set_email_sending.py     # Envío de correos
├── set_attachs_save_v6.py   # Procesador de adjuntos IMAP
├── set_socar_records_v1.py  # Records sender (autocontenido)
└── set_socar_resend_v1.py   # Reenvío (usa facade)
```

## Install

```bash
python -m venv venv
source venv/bin/activate      # Linux
.\venv\Scripts\activate       # Windows
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your credentials
```

## Usage

```bash
# Generate today's reports
cd source
python set_socar_reports_v2.py

# Test diario query (PRINT only, no generation)
python ../tests/test_diario_query.py

# Build .exe (Windows)
python ../build/build_exes.py
```

## Tests

```bash
pytest tests/ -v
# 58 tests — database, report_generator, facade, settings, utils, sql_processor
```

## Build (PyInstaller)

| Target | Source → .exe |
|--------|--------------|
| `send_attachs.exe` | `set_attachs_save_v6.py` |
| `send_records.exe` | `set_socar_records_v1.py` |
| `send_resend.exe` | `set_socar_resend_v1.py` + facade |

```bash
python build/build_exes.py
```

## Data Flow

```
soc_reports (DB)
  → get_check_reports()
    → get_socar_report()
      → get_data_client()
      → get_reports_available()
      → get_reports_platform()
      → get_data_events()
    → insert_table_* / simple_* (docx + graphs)
  → send_email() / get_email_approved()
```

---

<div align="center">

**Development by Angel Esquivel (Operations) — SOCAR Reports**

</div>
