# Python UV – Fast, Reproducible Python Dependency Management ⚡

> **UV** is a modern, ultra-fast Python package manager written in Rust.  
> It replaces `pip + virtualenv` with a **simpler, faster, and reproducible workflow**.

This repository demonstrates how to use **UV** for:
- Dependency management
- Virtual environments
- Dev vs production dependencies
- Team collaboration via Git

---

## Why UV?

- 🚀 10–100× faster than pip
- 🔒 Reproducible installs with lock files
- 🧰 Built-in virtual environment management
- 🧠 Modern dependency resolver
- 🔄 Easy migration from pip / Poetry / Conda
- 💾 Low memory footprint

---

## Installing UV

### macOS / Linux
```bash
curl -LsSf https://astral.sh/uv/install.sh | sudo sh
```

### Windows (PowerShell – Admin)
```powershell
irm https://astral.sh/uv/install.ps1 | iex
```

### Verify installation:
```bash
uv version
```

---

## Initializing a Project
```bash
uv init my-project
cd my-project
```

This creates:
```text
my-project/
├── pyproject.toml
├── uv.lock
├── .venv/
├── .python-version
├── README.md
└── hello.py
```

---

## Dependency Management

### Add production dependencies
```bash
uv add numpy pandas
```

### Add development dependencies
```bash
uv add --dev pytest black ruff
```

### Dependency locations:
- **Production deps** → `[project.dependencies]`
- **Dev deps** → `[tool.uv.dev-dependencies]`

---

## Dev vs Production Dependencies

### Production dependencies
Required to run the application. Examples:
- `numpy`
- `pandas`
- `fastapi`
- `requests`

### Development dependencies
Required only for development & testing. Examples:
- `pytest`
- `black`
- `ruff`
- `mypy`

---

## Installing Dependencies (UV equivalent of `pip install -r`)
```bash
uv sync
```
Production-only install:
```bash
uv sync --no-dev
```

---

## Running Python Code

### Recommended (no manual activation)
```bash
uv run python app.py
```

### Optional manual activation
```bash
source .venv/bin/activate
```
Deactivate:
```bash
deactivate
```

---

## Lock Files vs requirements.txt

### `uv.lock`
- Full dependency tree
- Exact versions
- Reproducible builds
- **Commit this file**

### Generate `requirements.txt` (for compatibility)
```bash
uv export -o requirements.txt
```
*Note: This includes all transitive dependencies by design.*

---

## Git Workflow (Team Collaboration)

### Commit these files:
- `pyproject.toml`
- `uv.lock`

### Do NOT commit:
- `.venv/`

### After cloning the repo:
```bash
uv sync
```
This is the UV equivalent of `pip install -r requirements.txt`.

---

## Common Command Mapping

| pip / virtualenv | UV |
| :--- | :--- |
| `python -m venv .venv` | `uv init` |
| `pip install pkg` | `uv add pkg` |
| `pip install -r requirements.txt` | `uv sync` |
| `pip uninstall pkg` | `uv remove pkg` |
| `pip freeze` | `uv export` |
| `source venv/bin/activate` | `uv run` |

---

## When to Use UV (and When Not To)

### Use UV when:
- You want fast installs
- You want reproducible environments
- You work in teams or CI/CD pipelines

### Avoid UV when:
- You require non-Python system packages (Conda may fit better)

### Key Takeaway 🧠
- `pyproject.toml` = what you want
- `uv.lock` = what you actually get

---

## Credits
This project and README are inspired by the excellent DataCamp tutorial:
👉 [https://www.datacamp.com/tutorial/python-uv](https://www.datacamp.com/tutorial/python-uv)
