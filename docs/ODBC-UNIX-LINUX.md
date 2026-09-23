# ODBC en socar-reports: Referencia de Dependencia de Sistema
<!-- 2026-09-14 · Angelo Esquivel · CyberSecurity -->
<!-- Backup: archivos .txt/.md NO requieren oc-backup al ser nuevos -->

## Contexto
`pyodbc` requiere una librería de sistema (libodbc.so.2) que **no viene incluida**
en los wheels de pip. Sin ella, `import pyodbc` falla.

---

## Comportamiento por plataforma

### Linux / Ubuntu / WSL2
- **ODBC no está incluido en el OS** a diferencia de Windows.
- pyodbc es un wrapper C sobre el driver manager ODBC.
- Los wheels publicados en PyPI **dependen de la lib de sistema** `libodbc.so.2`
  y NO la traen empaquetados (la descargan del entorno).
- Sin la lib, `import pyodbc` falla con:
  `ImportError: libodbc.so.2: cannot open shared object file: No such file or directory`
- **WSL2 corre Ubuntu real** → misma regla que Linux, necesita instalación manual.

**Instalación:**
```bash
sudo apt install unixodbc unixodbc-dev
```
Esto instala:
- `libodbc.so.2` (runtime, para que pyodbc pueda importar)
- `odbcinst.h` (headers, para compilar extensión si hace falta)

### Windows (nativo)
- ODBC **es parte del OS** (odbc32.dll, driver manager incluido).
- pyodbc se instala directo desde pip sin nada extra.
- No requiere unixODBC.

### macOS
- Viene con unixODBC (incluido en Command Line Tools / Homebrew).

---

## Drivers de SQL Server (capa adicional)
Los drivers de ODBC para SQL Server (`msodbcsql18`) son una **capa aparte**
del driver manager:

- **Linux:** `sudo apt install msodbcsql18` (desde repo Microsoft)
- **Windows:** instalar "ODBC Driver 18 for SQL Server" desde Microsoft
- **Verificar:** `pyodbc.drivers()` → lista los drivers instalados

En socar-reports, `pyodbc.drivers()` puede devolver `[]` si solo está instalado
el driver manager (unixODBC) pero no los drivers de SQL Server.

---

## Fuentes
- pyodbc docs: https://github.com/mkleehammer/pyodbc/wiki/Install
- Microsoft ODBC: https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server
