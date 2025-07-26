# Python Coding Standards

These standards are designed to promote readable, maintainable, and educational code.

## 1. Core Readability Standards

Readability is the highest priority. Code should be self-explanatory.

### Naming Conventions
-   **Variables**: Use descriptive, snake_case names (e.g., `user_email`, `retry_count`). Avoid single-letter or ambiguous names.
-   **Functions**: Use descriptive, snake_case verbs (e.g., `fetch_user_data`, `validate_input`).
-   **Classes**: Use PascalCase nouns (e.g., `WebScraper`, `DataProcessor`).
-   **Constants**: Use all-caps snake_case (e.g., `MAX_RETRIES`, `DEFAULT_TIMEOUT`).

### File and Function Structure
-   **Module Docstring**: Start every file with a docstring explaining its purpose and what can be learned from it.
-   **Imports**: Group imports in this order: 1. Standard library, 2. Third-party, 3. Local application.
-   **Function Docstrings**: All functions must have a Google-style docstring explaining what it does, its arguments (`Args`), what it returns (`Returns`), and a usage `Example`.
-   **Single Responsibility**: Each function should do one thing well. If a function is too long or complex, break it down.

## 2. Essential Coding Patterns

These patterns should be used consistently across all projects.

### Error Handling
-   **Use `try...except` blocks**: For any operation that might fail, especially file I/O and network requests.
-   **Be Specific**: Catch specific exceptions (`FileNotFoundError`, `ValueError`) instead of a generic `Exception` whenever possible.
-   **Provide Context**: Error messages should be helpful, explaining what went wrong and how to fix it.

**Basic Error Handling Pattern:**
```python
try:
    # Code that might fail
    response = requests.get(url, timeout=10)
    response.raise_for_status()  # Raises an exception for bad status codes
    return response.json()
except requests.RequestException as e:
    print(f"Error: Network request failed for {url}. Details: {e}")
    return None
```

### Configuration Management
-   **Use `.env` files**: Store secrets like API keys in a `.env` file. This file **MUST** be listed in `.gitignore`.
-   **Use a `config.py` file**: Centralize settings and load environment variables into a `Settings` class for easy access throughout the application.

**Example `config/settings.py`:**
```python
import os
from dotenv import load_dotenv

load_dotenv()

class Settings:
    GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")
    DEFAULT_TIMEOUT = int(os.getenv("DEFAULT_TIMEOUT", 10))
```

### Input Validation
-   Always validate data coming from external sources (user input, API responses, files).
-   Check for presence, type, and format before processing.

**Example Validation:**
```python
def process_user(user_data: dict):
    if not isinstance(user_data, dict):
        raise TypeError("user_data must be a dictionary.")

    email = user_data.get("email")
    if not email or "@" not in email:
        raise ValueError("A valid email is required.")

    # ... proceed with processing
```

## 3. Advanced (Optional) Best Practices

These topics should be introduced gradually as skill level increases. They are not required for beginner-level projects but represent good goals for growth.

### Custom Exceptions
For larger projects, defining a custom exception hierarchy can make error handling more organized.

```python
class ProjectError(Exception):
    """Base exception for this project."""
    pass

class DataValidationError(ProjectError):
    """Raised for issues with input data."""
    pass
```

### Type Hinting
Use type hints for all function signatures to improve clarity and allow for static analysis.

```python
from typing import List, Dict, Optional

def process_data(items: List[str]) -> Optional[Dict[str, int]]:
    # ... function implementation
    pass
```

### Versioning
For reusable tools, use Semantic Versioning (`MAJOR.MINOR.PATCH`) to manage changes and communicate them clearly.
