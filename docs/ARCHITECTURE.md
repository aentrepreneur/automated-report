<!-- Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
# Architecture — Automated Report

## Pipeline Pattern

```
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│ SOURCE  │──▶│ PROCESS │──▶│ RENDER  │──▶│ DELIVER │
└─────────┘   └─────────┘   └─────────┘   └─────────┘
     │             │             │             │
  SQL/DB       pandas DF     python-docx    Amazon SES
  YAML/JSON    chart data    python-pptx    IMAP fetch
  TXT          retry logic   matplotlib     file output
                             OPC metadata   batch queue
```

## SOC Reports (from the SOC reporting pipeline v2.0.0)

```
SQL Query → pyodbc → pandas DataFrame → matplotlib charts
  → python-docx (inline charts) → Amazon SES (HTML+attachment)
  → IMAP fetch (inbound)
```

Key modules:
- `source/settings.py` — Pydantic configuration
- `source/report_generator.py` — docx assembly
- `source/email_sender.py` — SES dispatch
- `source/attachment_processor.py` — IMAP response handling
- `source/graphs.py` — matplotlib chart generation (6 types)

## Doc Generation (from auto-docs v2.0.0)

```
YAML/JSON → reader.py → pipeline.py → builders/
  ├─ mop_builder.py       → MOP procedures
  ├─ ejecutivo_builder.py → Executive reports
  └─ pptx_builder.py      → PowerPoint presentations
```

Key modules:
- `src/reader.py` — multi-format input parser
- `src/pipeline.py` — orchestrator with builder dispatch
- `src/builders/` — template-driven builders per document type
- `src/handlers/` — section and content handlers
- `src/charts/` — matplotlib wrapper

<!-- End Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
