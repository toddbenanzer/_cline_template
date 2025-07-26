# Testing and Quality Standards for Personal Projects

## Testing Philosophy for Learning

Your testing approach should be simple, practical, and educational. We're not building enterprise software - we're learning Python while creating useful tools. Every test should teach you something about how your code works.

## Simple Testing Patterns

### Manual Testing First

```python
# Start with print-based testing
# File: /app/app_i_am_developing/tests/manual_test.py

def test_my_function_manually():
    """
    Manual testing with clear output.

    Learning points:
    - See exactly what your function does
    - Understand the flow of data
    - Catch obvious errors quickly
    """
    print("=== Testing My Function ===")

    # Test 1: Normal case
    print("\nTest 1: Normal input")
    result = my_function("normal input")
    print(f"Input: 'normal input'")
    print(f"Output: {result}")
    print(f"Expected: 'processed: normal input'")
    print(f"✅ PASS" if result == "processed: normal input" else "❌ FAIL")

    # Test 2: Empty input
    print("\nTest 2: Empty input")
    result = my_function("")
    print(f"Input: ''")
    print(f"Output: {result}")
    print(f"Expected: None")
    print(f"✅ PASS" if result is None else "❌ FAIL")

    # Test 3: Special characters
    print("\nTest 3: Special characters")
    result = my_function("test@#$%")
    print(f"Input: 'test@#$%'")
    print(f"Output: {result}")


if __name__ == "__main__":
    test_my_function_manually()
```

### Progressive Testing Approach

```python
# Level 1: Assert-based testing
def test_calculator_v1():
    """Basic testing with assert statements."""
    # Test addition
    result = add(2, 3)
    assert result == 5, f"Expected 5, got {result}"

    # Test with negatives
    result = add(-1, 1)
    assert result == 0, f"Expected 0, got {result}"

    print("✅ All calculator tests passed!")


# Level 2: Try-except for better error messages
def test_calculator_v2():
    """Testing with helpful error messages."""
    tests = [
        (2, 3, 5, "positive numbers"),
        (-1, 1, 0, "negative and positive"),
        (0, 0, 0, "zeros"),
        (0.1, 0.2, 0.3, "decimals")
    ]

    for a, b, expected, description in tests:
        try:
            result = add(a, b)
            assert abs(result - expected) < 0.0001  # Handle float precision
            print(f"✅ PASS: {description} ({a} + {b} = {result})")
        except AssertionError:
            print(f"❌ FAIL: {description} ({a} + {b} = {result}, expected {expected})")


# Level 3: Simple pytest tests
import pytest

def test_calculator_v3():
    """Pytest style tests."""
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

    # Test error cases
    with pytest.raises(TypeError):
        add("2", 3)  # Should fail with strings
```

### Real Data Testing

```python
# Test with actual data files
# File: /app/app_i_am_developing/tests/test_with_real_data.py

from pathlib import Path
import pandas as pd


def test_csv_processor():
    """Test CSV processing with real sample data."""
    # Use a small sample file for testing
    test_file = Path("/app/app_i_am_developing/data/test_samples/sample.csv")

    # Create test data if it doesn't exist
    if not test_file.exists():
        test_file.parent.mkdir(parents=True, exist_ok=True)
        sample_data = pd.DataFrame({
            'name': ['Alice', 'Bob', 'Charlie'],
            'age': [25, 30, 35],
            'city': ['NYC', 'LA', 'Chicago']
        })
        sample_data.to_csv(test_file, index=False)
        print(f"Created test file: {test_file}")

    # Test loading
    processor = CSVProcessor()
    df = processor.load_csv(test_file)

    print(f"✅ Loaded {len(df)} rows")
    print(f"✅ Columns: {list(df.columns)}")

    # Test processing
    processed = processor.process(df)

    print(f"✅ Processed data shape: {processed.shape}")

    # Visual verification
    print("\nProcessed data preview:")
    print(processed.head())

    return processed


def test_web_scraper_offline():
    """Test scraper with saved HTML files."""
    # Save example HTML for offline testing
    test_html = """
    <html>
        <head><title>Test Page</title></head>
        <body>
            <h1>Test Article</h1>
            <p>This is test content.</p>
            <a href="/link1">Link 1</a>
            <a href="/link2">Link 2</a>
        </body>
    </html>
    """

    # Test parsing without network calls
    scraper = WebScraper()
    data = scraper.parse_html(test_html)

    print("Extracted data:")
    print(f"- Title: {data.get('title')}")
    print(f"- Links: {data.get('links')}")
    print(f"- Content: {data.get('content')[:50]}...")

    # Verify expectations
    assert data['title'] == 'Test Page'
    assert len(data['links']) == 2
    print("✅ Scraper parsing works correctly!")
```

## Quality Checks for Personal Projects

### Code Quality Checklist

```python
# File: /app/app_i_am_developing/scripts/quality_check.py
"""
Simple quality checks for your code.
"""

from pathlib import Path
import ast


def check_code_quality(py_file):
    """Basic code quality checks."""
    print(f"\n=== Checking {py_file.name} ===")

    with open(py_file, 'r') as f:
        content = f.read()

    # Check 1: File has docstring
    if '"""' in content[:200] or "'''" in content[:200]:
        print("✅ Has module docstring")
    else:
        print("⚠️  Missing module docstring")

    # Check 2: No very long lines
    long_lines = []
    for i, line in enumerate(content.split('\n'), 1):
        if len(line) > 100:
            long_lines.append(i)

    if long_lines:
        print(f"⚠️  Long lines (>100 chars) at: {long_lines[:5]}")
    else:
        print("✅ No overly long lines")

    # Check 3: Has error handling
    if 'try:' in content:
        print("✅ Uses error handling")
    else:
        print("⚠️  Consider adding error handling")

    # Check 4: Reasonable file size
    lines = len(content.split('\n'))
    if lines > 500:
        print(f"⚠️  Large file ({lines} lines) - consider splitting")
    else:
        print(f"✅ Reasonable size ({lines} lines)")


def check_project_quality():
    """Check overall project quality."""
    project_root = Path("/app/app_i_am_developing")

    print("=== Project Quality Check ===")

    # Check 1: README exists
    if (project_root / "README.md").exists():
        print("✅ README.md exists")
    else:
        print("❌ Missing README.md")

    # Check 2: Requirements file
    if (project_root / "requirements.txt").exists():
        print("✅ requirements.txt exists")
    else:
        print("❌ Missing requirements.txt")

    # Check 3: .gitignore
    if (project_root / ".gitignore").exists():
        print("✅ .gitignore exists")
    else:
        print("⚠️  Missing .gitignore")

    # Check 4: Test directory
    if (project_root / "tests").exists():
        test_count = len(list((project_root / "tests").glob("*.py")))
        print(f"✅ Tests directory exists ({test_count} test files)")
    else:
        print("⚠️  No tests directory")

    # Check all Python files
    print("\n=== Code Quality ===")
    for py_file in project_root.rglob("*.py"):
        if "__pycache__" not in str(py_file):
            check_code_quality(py_file)


if __name__ == "__main__":
    check_project_quality()
```

### Performance Testing for Personal Use

```python
# Simple performance checks
# File: /app/app_i_am_developing/tests/performance_check.py

import time
import psutil
import os


def measure_performance(func, *args, **kwargs):
    """
    Measure function performance.

    Learning points:
    - Execution time
    - Memory usage
    - Resource consumption
    """
    # Get process
    process = psutil.Process(os.getpid())

    # Memory before
    memory_before = process.memory_info().rss / 1024 / 1024  # MB

    # Time execution
    start_time = time.time()
    result = func(*args, **kwargs)
    end_time = time.time()

    # Memory after
    memory_after = process.memory_info().rss / 1024 / 1024  # MB

    # Report
    print(f"\n=== Performance Report: {func.__name__} ===")
    print(f"⏱️  Execution time: {end_time - start_time:.3f} seconds")
    print(f"💾 Memory used: {memory_after - memory_before:.1f} MB")
    print(f"📊 Current total memory: {memory_after:.1f} MB")

    return result


def test_data_processing_performance():
    """Test performance of data operations."""
    # Create test data
    import pandas as pd

    print("Creating test dataset...")
    df = pd.DataFrame({
        'id': range(10000),
        'value': [i * 2.5 for i in range(10000)],
        'category': ['A', 'B', 'C', 'D'] * 2500
    })

    # Test different operations
    def process_with_loop():
        """Slow method with loop."""
        results = []
        for _, row in df.iterrows():
            results.append(row['value'] * 2)
        return results

    def process_with_vectorization():
        """Fast method with pandas."""
        return (df['value'] * 2).tolist()

    # Compare performance
    print("\nTesting loop method:")
    measure_performance(process_with_loop)

    print("\nTesting vectorized method:")
    measure_performance(process_with_vectorization)

    print("\n💡 Learning: Vectorized operations are much faster!")
```

## Debugging Strategies

### Print-Based Debugging

```python
def debug_with_prints(data):
    """
    Debugging with strategic print statements.

    Learning approach:
    1. Print inputs
    2. Print intermediate steps
    3. Print outputs
    4. Print types and shapes
    """
    print(f"\n🔍 DEBUG: Starting function")
    print(f"   Input type: {type(data)}")
    print(f"   Input value: {data[:50] if len(str(data)) > 50 else data}")

    # Step 1
    processed = step1(data)
    print(f"\n🔍 DEBUG: After step1")
    print(f"   Type: {type(processed)}")
    print(f"   Length: {len(processed) if hasattr(processed, '__len__') else 'N/A'}")

    # Step 2
    result = step2(processed)
    print(f"\n🔍 DEBUG: After step2")
    print(f"   Result: {result}")

    print(f"\n🔍 DEBUG: Function complete")
    return result
```

### Error Investigation Pattern

```python
def investigate_error():
    """
    Systematic error investigation.
    """
    try:
        # Suspicious code
        result = risky_operation()
    except Exception as e:
        print(f"\n❌ Error occurred: {type(e).__name__}")
        print(f"   Message: {str(e)}")

        # Get more context
        import traceback
        print("\n📋 Full traceback:")
        traceback.print_exc()

        # Check common causes
        print("\n🔍 Investigating common causes:")

        # File issues?
        if "FileNotFoundError" in str(type(e)):
            print("   - Check file paths")
            print("   - Current directory:", os.getcwd())
            print("   - Files in directory:", os.listdir('.'))

        # Type issues?
        elif "TypeError" in str(type(e)):
            print("   - Check variable types")
            print("   - Are you passing the right arguments?")

        # Add debug info
        print("\n💡 Debug suggestions:")
        print("   1. Add print statements before the error")
        print("   2. Check input data")
        print("   3. Try with simpler test case")
```

## Continuous Improvement

### Learning from Errors

```python
# File: /app/app_i_am_developing/tests/error_journal.py
"""
Keep track of errors and solutions for learning.
"""

import json
from datetime import datetime
from pathlib import Path


class ErrorJournal:
    """Track errors and solutions for learning."""

    def __init__(self):
        self.journal_file = Path("/app/app_i_am_developing/docs/error_journal.json")
        self.load_journal()

    def load_journal(self):
        """Load existing journal."""
        if self.journal_file.exists():
            with open(self.journal_file, 'r') as f:
                self.entries = json.load(f)
        else:
            self.entries = []

    def add_error(self, error_type, description, solution, lesson_learned):
        """Add an error experience."""
        entry = {
            'date': datetime.now().isoformat(),
            'error_type': error_type,
            'description': description,
            'solution': solution,
            'lesson_learned': lesson_learned,
            'tags': self.extract_tags(description)
        }

        self.entries.append(entry)
        self.save_journal()

        print(f"✅ Added to error journal: {error_type}")

    def extract_tags(self, description):
        """Extract learning tags from description."""
        tags = []

        # Common Python topics
        topics = ['import', 'TypeError', 'ValueError', 'FileNotFound',
                 'index', 'key', 'attribute', 'encoding', 'api', 'timeout']

        for topic in topics:
            if topic.lower() in description.lower():
                tags.append(topic)

        return tags

    def save_journal(self):
        """Save journal to file."""
        self.journal_file.parent.mkdir(parents=True, exist_ok=True)

        with open(self.journal_file, 'w') as f:
            json.dump(self.entries, f, indent=2)

    def search_similar_errors(self, keyword):
        """Find similar past errors."""
        matches = []

        for entry in self.entries:
            if keyword.lower() in entry['description'].lower():
                matches.append(entry)

        return matches

    def get_summary(self):
        """Get learning summary."""
        print(f"\n=== Error Journal Summary ===")
        print(f"Total errors logged: {len(self.entries)}")

        # Count by type
        error_types = {}
        for entry in self.entries:
            error_type = entry['error_type']
            error_types[error_type] = error_types.get(error_type, 0) + 1

        print("\nMost common errors:")
        for error_type, count in sorted(error_types.items(),
                                      key=lambda x: x[1], reverse=True):
            print(f"  - {error_type}: {count} times")

        # Recent lessons
        print("\nRecent lessons learned:")
        for entry in self.entries[-3:]:
            print(f"  - {entry['lesson_learned']}")


# Usage example
if __name__ == "__main__":
    journal = ErrorJournal()

    # Add an error experience
    journal.add_error(
        error_type="FileNotFoundError",
        description="Tried to open config.json but file didn't exist",
        solution="Check if file exists before opening, create default if missing",
        lesson_learned="Always use Path.exists() before opening files"
    )

    # Search for similar errors
    similar = journal.search_similar_errors("file")
    print(f"\nFound {len(similar)} similar file-related errors")

    # Get summary
    journal.get_summary()
```

### Code Review for Learning

```python
def self_code_review(file_path):
    """
    Review your own code for learning.

    Questions to ask:
    1. Can I understand this in 3 months?
    2. Are variable names clear?
    3. Is error handling adequate?
    4. Could someone else use this?
    """
    print(f"\n=== Self Code Review: {file_path} ===")

    review_checklist = [
        ("Readability", "Can I understand each function's purpose?"),
        ("Naming", "Do variable names describe their content?"),
        ("Comments", "Are complex parts explained?"),
        ("Error Handling", "What happens when things go wrong?"),
        ("Testing", "How do I know this works?"),
        ("Documentation", "Could someone else use this?"),
        ("Learning", "What Python concepts did I use?"),
        ("Improvements", "What would I do differently now?")
    ]

    for category, question in review_checklist:
        response = input(f"\n{category}: {question} (y/n/partial): ").lower()

        if response == 'n':
            improvement = input("   How would you improve it? ")
            print(f"   📝 TODO: {improvement}")
        elif response == 'partial':
            improvement = input("   What needs work? ")
            print(f"   📝 TODO: {improvement}")
        else:
            print(f"   ✅ Good!")

    print("\n=== Review Complete ===")
    print("Remember: Every code review is a learning opportunity!")
```

## Testing Commands Quick Reference

### Basic Testing Commands

```bash
# Run manual tests
python tests/manual_test.py

# Run specific test function
python -c "from tests.test_scraper import test_scraper; test_scraper()"

# Run with pytest (if installed)
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src

# Quick function test
python -i src/my_module.py
>>> test_my_function()

# Performance test
python tests/performance_check.py

# Quality check
python scripts/quality_check.py
```

### Testing Workflow

```bash
# 1. Write code
# 2. Manual test
python -i src/new_feature.py
>>> # Test interactively

# 3. Create test file
echo "# Test for new feature" > tests/test_new_feature.py

# 4. Run test
python tests/test_new_feature.py

# 5. Document what you learned
echo "Learned: [concept]" >> docs/learning_log.md
```

Remember: Testing is about learning how your code behaves, not about achieving 100% coverage. Every test teaches you something about Python and your program!
