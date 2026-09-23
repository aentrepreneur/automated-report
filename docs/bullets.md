# Bullets Review — SOCAR Reports v2.0.0

## Decisiones Implementadas

### Timezone
- `settings.py` expone `TIMEZONE = ZoneInfo("America/Mexico_City")`
- Las funciones originales usan `datetime.now()` (naive). Pendiente migrar a `now_local()`.
- **Status**: documentado, no aplicado (backward compat preferida por ahora).

### Email Validation
- `pydantic[email]` + `email-validator` instalado (`pip install`).
- Validación `EmailStr` no activada en modelos (`MailSettings.username` es AWS key, no email).
- `ImapSettings.username` podría usar `EmailStr` cuando hay valor.

### Windows Paths
- Todos los módulos nuevos usan `pathlib.Path`.
- Rutas como `C:\TRSSQL\...` existen solo en `set_attachs_save_v6.py` (legacy intacto).

### Logging
- `RotatingFileHandler` en logger `socar_app` + root logger `basicConfig` (compatible legacy).
- Probado: `set_socar_resend_v1.py` importa facade y loguea sin conflictos.

### Credenciales SMTP en set_email_sending.py
- Aún hardcodeadas en `set_email_sending.py:67,132` (archivo legacy no refactorizado).
- `.env` sí las tiene configuradas para los módulos nuevos.

### PyInstaller
- 3 targets con `build/build_exes.py`.
- `send_resend.exe` necesita `--hidden-import` de todos los submódulos del facade.

## 58 Tests (7 suites)

| Suite | Tests | Cobertura |
|-------|-------|-----------|
| test_settings | 3 | Modelos Pydantic |
| test_utils | 8 | get_related_text/range, semana |
| test_sql_processor | 8 | SQL-like sobre DataFrames |
| test_db_connectivity | 3 | Conexión DB mockeada |
| test_database | 22 | DB data_platform, HPC, events, upload_log, URL parsing |
| test_report_generator | 10 | Orquestación mockeada (available, platform, events, check, socar) |
| test_facade | 5 | Re-exportación de símbolos, import * |

## Testing Framework (v2.1.0)

### CLI Unificada
```
python testings/cli.py [--sandbox] [--dry-run] [--step] [--capture DIR] <domain> <action> [flags]
```
- `csv`: siem, merge, merge-bi, generate, validate
- `file`: imap (--mailbox, --report, --ext, --date-from, --date-to, --sender, --no-mark-seen), docx, unzip, mock-imap, capture-smtp
- `db`: test-connection, client, tables, export, upload, init-mock, seed, query
- `pipeline`: run (multi-step: fetch→convert→validate→load→report→send)

### Sandbox Mode
- `--sandbox` activa infraestructura mock: SQLite en vez de SQL Server, .eml locales en vez de IMAP real, captura a archivo en vez de SMTP
- Generadores de datos dummy: `csv generate`, `db seed`, `file mock-imap`

### Dry-Run Mode
- `--dry-run` en comandos individuales: muestra intento sin ejecutar
- En pipeline: cada paso reporta lo que haría sin conexiones reales

### Archivos Nuevos
| Archivo | Propósito |
|---------|-----------|
| `testings/cli.py` | Entry point unificado con argparse |
| `testings/csv_tools.py` | Procesamiento CSV (SIEM, merge, merge-bi) |
| `testings/file_tools.py` | Operaciones archivo (IMAP, docx, unzip) |
| `testings/db_tools.py` | Operaciones DB (client, tables, export, upload) |
| `testings/sandbox/fixtures.py` | Generadores de datos mock |
| `.env.test` | Config de prueba (localhost) |
| `testings/README.md` | Documentación completa del CLI |
| `--help` | 3 niveles: dominio, sub-accion, comando. Todos los args documentados |

## Pendiente

- [ ] Migrar `datetime.now()` → `now_local()` con ZoneInfo
- [ ] Activar `EmailStr` en `ImapSettings.username`
- [ ] Mover credenciales SMTP de `set_email_sending.py` a `.env`
- [ ] Probar generación real de reportes desde Windows 11
- [ ] Verificar PyInstaller build en Windows
- [ ] Pipeline `send` real: integrar `set_email_sending.py`
- [ ] Pipeline `report` real: integrar `report_generator.py` con mock data
