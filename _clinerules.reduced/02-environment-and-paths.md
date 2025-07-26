# Environment, Paths, and Project Setup

This document defines the standard for the development environment, ensuring consistency and preventing path-related errors.

## 1. The Project Root Placeholder: `{PROJECT_ROOT}`

-   **Definition**: The placeholder `{PROJECT_ROOT}` refers to the project's root directory inside the container.
-   **Current Value**: `c:/developer/_cline_template`
-   **Usage**: This placeholder **MUST** be used for all file paths to ensure portability and prevent errors.

## 2. Path Handling with `pathlib`

All file and directory paths **MUST** be handled using Python's `pathlib` module. This is a non-negotiable standard.

**Correct Usage:**
```python
from pathlib import Path

# Always start from the project root
project_root = Path("{PROJECT_ROOT}")

# Build paths from the root
data_dir = project_root / "data"
config_file = project_root / "config" / "settings.py"
```

**Incorrect Usage (To be avoided):**
```python
# DO NOT use string concatenation
data_dir = "{PROJECT_ROOT}" + "/data"

# DO NOT use relative paths that can be ambiguous
data_dir = Path("./data")
```

## 3. Standard Project Structure

All projects should follow this standard directory structure. A script is provided to set this up automatically.

**Standard Directories:**
```
{PROJECT_ROOT}/
├── .devcontainer/   # VS Code Remote Container config
├── .vscode/         # VS Code workspace settings
├── config/          # Configuration files (e.g., settings.py)
├── data/
│   ├── input/       # Raw input data
│   └── output/      # Processed data and results
├── docs/            # Project documentation
├── examples/        # Example scripts showing usage
├── memory-bank/     # Cline's memory files
├── notebooks/       # Jupyter notebooks for exploration
├── scripts/         # Automation and utility scripts
├── src/             # Main source code
└── tests/           # Test files
```

## 4. Environment Validation

Before starting any significant work, a quick environment validation should be performed to ensure the container is set up correctly.

**Key Validation Checks:**
1.  **Working Directory**: Must be `{PROJECT_ROOT}`.
2.  **Python Version**: Should be 3.12+.
3.  **`src` Directory**: The `{PROJECT_ROOT}/src` directory must exist.
4.  **Write Permissions**: Must have write access to `{PROJECT_ROOT}/data/output`.

**Validation Command Example:**
```bash
# Run this in the terminal at the start of a session
python scripts/validate_env.py
```

## 5. Project Setup Script

A utility script should be available at `{PROJECT_ROOT}/scripts/setup_project.py` to create the standard directory and file structure for a new project.

**Core functionality of `setup_project.py`:**
-   Creates all standard directories.
-   Creates essential boilerplate files like `README.md`, `requirements.txt`, `.gitignore`, and `.env.example`.
-   Is idempotent, meaning it can be run multiple times without causing errors.
