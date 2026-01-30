# Python UV – Fast, Reproducible Python Dependency Management ⚡

> **UV** is a modern, ultra-fast Python package manager written in Rust.  
> It replaces `pip + virtualenv` with a **single, unified tool** that provides a **simpler, faster, and reproducible workflow**.

This repository demonstrates how to use **UV** for:
- Dependency management focusing on speed and accuracy.
- Virtual environment creation and automatic management.
- Separation of development and production dependencies.
- Team collaboration via Git using deterministic lock files.

---

## Why UV?

- 🚀 **Extreme Speed:** 10–100× faster than pip. It uses a **global cache** to avoid re-downloading packages and employs **content-addressable storage** for "no-copy" installs (using hardlinks or reflinks).
- 🔒 **Reproducibility:** Every installation is backed by a `uv.lock` file, ensuring every developer and CI environment uses the exact same package versions.
- 🧰 **Unified Tooling:** Built-in virtual environment management means you no longer need `venv` or `virtualenv` as separate steps.
- 🧠 **Advanced Resolver:** A modern dependency resolver that can handle complex constraints and provides clear error messages when conflicts occur.
- 🔄 **Easy Migration:** Native support for `pyproject.toml` makes migrating from pip, Poetry, or Conda straightforward.
- 💾 **Efficiency:** Significantly lower memory footprint during installation compared to traditional tools.

---

## Installing UV

### macOS / Linux
```bash
curl -LsSf https://astral.sh/uv/install.sh | sudo sh
```
*The script automatically detects your shell and adds `uv` to your PATH.*

### Windows (PowerShell – Admin)
```powershell
irm https://astral.sh/uv/install.ps1 | iex
```

### Verify installation:
```bash
uv version
```
*Keep UV updated by running `uv self update` regularly.*

---

## Initializing a Project
```bash
uv init my-project
cd my-project
```

This creates a structured boilerplate:
- `pyproject.toml`: The standard-compliant file where you define project metadata and dependencies.
- `uv.lock`: A deterministic lockfile that records the exact versions of every package in your tree.
- `.venv/`: A local virtual environment managed entirely by UV.
- `.python-version`: A file that tells UV which Python version to use for this specific project.
- `hello.py`: A simple entry point to test your environment.

---

## Dependency Management

### Add production dependencies
```bash
uv add numpy pandas
```
*This command updates `pyproject.toml`, fetches the packages, and updates the `uv.lock` file in one step.*

### Add development dependencies
```bash
uv add --dev pytest black ruff
```
*Dev dependencies are strictly for local testing and linting, keeping your final production environment lightweight.*

### Dependency locations:
- **Production deps** → Located under `[project.dependencies]` in `pyproject.toml`.
- **Dev deps** → Managed under `[tool.uv.dev-dependencies]` for clear separation.

---

## Dev vs Production Dependencies

### Production dependencies
Essential packages required for the core application logic to function.
- `numpy` & `pandas`: Data manipulation and numerical analysis.
- `fastapi`: Serving the application via API.
- `requests`: Handling external HTTP communication.

### Development dependencies
Tools used for code quality, testing, and debugging. They are never installed in production.
- `pytest`: For running unit and integration tests.
- `black` & `ruff`: For automated code formatting and linting.
- `mypy`: For static type checking of your Python code.

---

## Installing Dependencies (UV equivalent of `pip install -r`)
```bash
uv sync
```
*The `sync` command ensures your local `.venv` perfectly matches the states defined in `pyproject.toml` and `uv.lock`. It will even remove packages no longer listed.*

Production-only install (ideal for Docker images):
```bash
uv sync --no-dev
```

---

## Running Python Code

### Recommended (no manual activation)
```bash
uv run python app.py
```
*`uv run` automatically ensures the environment is up-to-date and runs the script within the context of your project's virtual environment.*

### Optional manual activation
If you prefer a traditional workflow:
```bash
source .venv/bin/activate
```
Deactivate when finished:
```bash
deactivate
```

---

## Lock Files vs requirements.txt

### `uv.lock`
- **Full Tree:** Contains every dependency, including sub-dependencies (transitive).
- **Security:** Includes hashes for every package to prevent supply-chain tampering.
- **Portability:** Designed to be cross-platform, working across macOS, Linux, and Windows.
- **Strategy:** Always **commit this file** to your repository.

### Generate `requirements.txt` (for compatibility)
If you need to ship to an environment that only supports pip:
```bash
uv export -o requirements.txt
```
*Note: The export generates a "frozen" list containing every single package in the dependency graph.*

---

## Git Workflow (Team Collaboration)

### Commit these files:
- `pyproject.toml`: Defines the "rules" of your project.
- `uv.lock`: Bridges the gap between rules and reality by pinning exact versions.

### Do NOT commit:
- `.venv/`: Local environments are machine-specific and should be rebuilt from the lock file.

### After cloning the repo:
```bash
uv sync
```
*This single command replaces the old `pip install -r requirements.txt` routine, setting up the exact environment used by your teammates.*

---

## Common Command Mapping

| Action | Traditional (pip/venv) | UV Equivalent |
| :--- | :--- | :--- |
| **Initialize Project** | `python -m venv .venv` | `uv init` |
| **Add Package** | `pip install pkg` | `uv add pkg` |
| **Install Project** | `pip install -r requirements.txt` | `uv sync` |
| **Remove Package** | `pip uninstall pkg` | `uv remove pkg` |
| **Generate Lockfile** | `pip freeze > reqs.txt` | `uv export` |
| **Execute Code** | `source venv/bin/activate && python ...` | `uv run` |

---

## When to Use UV (and When Not To)

### Use UV when:
- **Speed Matters:** You want near-instant installation times in dev and CI/CD.
- **Environment Integrity:** You need to ensure every developer has the exact same setup.
- **Modern Workflow:** You prefer a single tool that handles everything from Python versions to package locking.

### Avoid UV when:
- **System-level Requirements:** You require complex non-Python system libraries (where Conda/Mamba's multi-language support excels).

### Key Takeaway 🧠
- **`pyproject.toml` = Your Intent:** This is where you declare your high-level requirements (e.g., `pandas>=2.0`). It defines the boundaries and goals of your project in a human-readable format.
- **`uv.lock` = The Reality:** This is the machine-generated, exhaustive map of every single package version and sub-dependency used. It ensures that "it works on my machine" translates to "it works on everyone's machine."
- **The Synergy:** By splitting "what I want" from "exactly what I have," UV allows your project to be flexible enough to upgrade while remaining rigid enough to be perfectly reproducible in production or CI/CD pipelines.

---

## Credits
This project and README are inspired by the excellent DataCamp tutorial:
👉 [https://www.datacamp.com/tutorial/python-uv](https://www.datacamp.com/tutorial/python-uv)
