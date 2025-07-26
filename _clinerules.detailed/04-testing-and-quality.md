# Testing and Quality Standards

This document outlines a simple, practical, and educational approach to testing for personal projects. The goal is to ensure code works as expected and to deepen understanding of its behavior.

## 1. Testing Philosophy: Test for Learning

-   **Start Simple**: Begin with manual, print-based tests before moving to automated checks.
-   **Test for Understanding**: Every test should answer a question about the code, such as "What happens if the input is empty?" or "Does it handle network errors correctly?"
-   **Practicality Over Coverage**: Focus on testing critical paths and likely failure points. 100% test coverage is not a primary goal.

## 2. The Progressive Testing Workflow

Testing should follow the same progressive pattern as development.

### Level 1: Manual Testing with `print()`
This is the first and most important step. Use `print()` statements to see exactly what your code is doing with different inputs.

**Example Manual Test Script (`tests/manual_test_scraper.py`):**
```python
from src.scraper import scrape_website

print("--- Testing Scraper ---")

# Test 1: Valid URL
print("\n[Test 1: Valid URL]")
result = scrape_website("http://example.com")
if result:
    print(f"✅ PASS: Got title: '{result.title.text}'")
else:
    print("❌ FAIL: Did not get a result.")

# Test 2: Invalid URL
print("\n[Test 2: Invalid URL]")
result = scrape_website("http://invalid-url-that-does-not-exist.com")
if not result:
    print("✅ PASS: Correctly handled invalid URL.")
else:
    print("❌ FAIL: Should have returned None.")
```

### Level 2: Assert-Based Testing
Once manual tests pass, formalize them using `assert` statements. This makes tests self-verifying.

**Example Assert-Based Test (`tests/test_calculator.py`):**
```python
from src.calculator import add

def test_addition():
    """Verify the add function works correctly."""
    assert add(2, 3) == 5, "Test with positive numbers failed."
    assert add(-1, 1) == 0, "Test with negative and positive failed."
    assert add(0, 0) == 0, "Test with zeros failed."
    print("✅ All addition tests passed!")

# To run this, you would execute the function.
# For more advanced use, a test runner like pytest would discover and run this.
```

### Level 3: Testing with Real Data
For data processing or scraping, test with small, representative samples of real data.

-   **Scrapers**: Save a copy of a target webpage's HTML to a file in `tests/fixtures/`. Your test can then parse the local file, avoiding network calls and ensuring consistency.
-   **Data Processors**: Create small CSV or JSON files in `tests/fixtures/` that cover normal cases, edge cases, and malformed data.

## 3. Simple Quality Checks

A simple script can help maintain overall project quality.

**Example Quality Check Script (`scripts/quality_check.py`):**
```python
from pathlib import Path

def check_project_quality():
    """Run a series of simple quality checks."""
    project_root = Path("{PROJECT_ROOT}")
    print("--- Project Quality Check ---")

    # Check 1: README exists
    if (project_root / "README.md").exists():
        print("✅ README.md found.")
    else:
        print("❌ Missing README.md file.")

    # Check 2: requirements.txt exists
    if (project_root / "requirements.txt").exists():
        print("✅ requirements.txt found.")
    else:
        print("❌ Missing requirements.txt file.")

    # Check 3: .env file is ignored
    gitignore_content = (project_root / ".gitignore").read_text()
    if ".env" in gitignore_content:
        print("✅ .env is correctly ignored in .gitignore.")
    else:
        print("⚠️  .env file is not in .gitignore. This is a security risk.")

if __name__ == "__main__":
    check_project_quality()
```

## 4. Debugging Strategy: Isolate and Observe

When an error occurs:
1.  **Isolate the Problem**: Create the smallest possible piece of code that reproduces the error.
2.  **Observe with `print`**: Add `print()` statements to observe the state of variables right before the error occurs. Check types (`print(type(my_var))`) and values.
3.  **Consult the `ErrorJournal`**: Create and maintain an `ErrorJournal` in your `docs` folder to log problems and their solutions, helping you learn from mistakes.
