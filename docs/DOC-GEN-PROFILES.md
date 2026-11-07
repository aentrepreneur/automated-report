<!-- Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
# Document Generation Profiles

## Profile: MOP (Procedure Manual)

| Aspect | Detail |
|--------|--------|
| Output | .docx |
| Sections | Objective, Scope, Responsibilities, Procedure, References |
| Metadata | OPC/Dublin Core (UUID, language, subject) |
| Input | YAML with procedure steps |

## Profile: Ejecutivo (Executive Report)

| Aspect | Detail |
|--------|--------|
| Output | .docx |
| Sections | Summary, Analysis, Findings, Recommendations, Appendix |
| Charts | Bar, pie, line, scatter (configurable placement) |
| Input | JSON with data tables |

## Profile: Presentacion (PowerPoint)

| Aspect | Detail |
|--------|--------|
| Output | .pptx |
| Slides | Title, Agenda, Content (N), Summary, Q&A |
| Charts | 6 chart types with quadrant grid positioning |
| Input | YAML with slide definitions |

## Chart Types

| Type | Variants | Supported In |
|------|----------|-------------|
| Bar | Vertical, Horizontal | DOCX, PPTX |
| Line | Single, Multi-series | DOCX, PPTX |
| Pie | Standard, Exploded | DOCX, PPTX |
| Scatter | Points, Connected | DOCX, PPTX |
| Histogram | Frequency | DOCX |
| Multi_line | Comparison | DOCX, PPTX |

## Builder Pattern

```
                    ┌──────────────┐
                    │  Pipeline    │
                    │ (orchestrator)│
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌─────────┐ ┌──────────┐ ┌──────────┐
        │ MOP     │ │ Ejecutivo│ │ PPTX     │
        │ Builder │ │ Builder  │ │ Builder  │
        └─────────┘ └──────────┘ └──────────┘
              │            │            │
              ▼            ▼            ▼
        python-docx   python-docx   python-pptx
```

Adding a new profile requires only a new builder class — no modifications to the pipeline core.

<!-- End Development By Angel Esquivel (Operations) [AUTOMATED-REPORT 2026] -->
