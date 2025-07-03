# Simple Testing and Quality for Personal Projects

## Testing Philosophy for Personal Use

### Why Test Personal Code?
- **Catch mistakes early**: Find problems before they affect your data
- **Build confidence**: Know your code works when you need it
- **Learn from failures**: Understand what breaks and why
- **Save time**: Quick tests beat manual checking every time

### Keep Testing Simple
- **Test what matters**: Focus on code that processes important data
- **Manual testing first**: Run with real data before automating
- **Basic validation**: Simple checks are better than no checks
- **Readable tests**: Tests should be easy to understand and modify

## Basic Testing in Docker Environment

### Simple Test Structure
```python
# tests/test_basic.py
"""Basic tests for personal application."""

import sys
from pathlib import Path

# Add src to Python path (for Docker environment)
sys.path.insert(0, '/app/app_i_am_developing/src')

from core.processor import DataProcessor

def test_data_processor():
    """Test basic data processing."""
    processor = DataProcessor()

    # Test with simple, realistic data
    test_data = [
        {'name': 'John', 'value': 100},
        {'name': 'Jane', 'value': 200}
    ]

    results = processor.process_data(test_data)

    # Simple checks
    assert len(results) == 2, f"Expected 2 results, got {len(results)}"
    assert results[0]['NAME'] == 'JOHN', f"Expected 'JOHN', got {results[0]['NAME']}"

    print("✓ Data processor test passed")

def test_file_processing():
    """Test file processing with temporary file."""
    import json
    import tempfile

    processor = DataProcessor()

    # Create temporary test file
    test_data = [{'name': 'test', 'value': 42}]

    with tempfile.NamedTemporaryFile(mode='w', suffix='.json', delete=False) as f:
        json.dump(test_data, f)
        test_file = f.name

    try:
        # Test processing
        results = processor.process_file(test_file)

        assert len(results) == 1, "Should process one item"
        assert results[0]['NAME'] == 'TEST', "Should uppercase name"

        print("✓ File processing test passed")

    finally:
        # Clean up
        Path(test_file).unlink()

def run_all_tests():
    """Run all tests."""
    print("Running basic tests...")

    try:
        test_data_processor()
        test_file_processing()
        print("\n🎉 All tests passed!")
        return True
    except AssertionError as e:
        print(f"\n❌ Test failed: {e}")
        return False
    except Exception as e:
        print(f"\n💥 Unexpected error: {e}")
        return False

if __name__ == "__main__":
    success = run_all_tests()
    sys.exit(0 if success else 1)
```

### Docker Test Runner
```bash
#!/bin/bash
# run_tests.sh - Simple test runner for Docker

echo "Running tests in Docker container..."

# Make sure container is running
docker-compose up -d

# Run tests
docker-compose exec app python -m app_i_am_developing.tests.test_basic

# Check exit code
if [ $? -eq 0 ]; then
    echo "✅ All tests passed!"
else
    echo "❌ Some tests failed!"
fi
```

## Manual Testing Patterns

### Interactive Testing
```python
# examples/interactive_test.py
"""Interactive testing for manual verification."""

from pathlib import Path
import sys

# Add src to path
sys.path.insert(0, '/app/app_i_am_developing/src')

from core.processor import DataProcessor

def interactive_test():
    """Test interactively with user input."""
    processor = DataProcessor()

    print("=== Interactive Testing ===")

    # Test with user-provided data
    while True:
        test_input = input("Enter test data (or 'quit'): ")
        if test_input.lower() == 'quit':
            break

        try:
            # Simple processing
            result = processor.process_item(test_input)
            print(f"Result: {result}")
        except Exception as e:
            print(f"Error: {e}")

    print("Interactive testing finished!")

def test_with_sample_files():
    """Test with actual sample files."""
    examples_dir = Path('/app/app_i_am_developing/examples')
    processor = DataProcessor()

    # Look for sample files
    sample_files = list(examples_dir.glob('sample_*'))

    if not sample_files:
        print("No sample files found in examples/")
        return

    for sample_file in sample_files:
        print(f"\nTesting with {sample_file.name}:")

        try:
            results = processor.process_file(sample_file)
            print(f"  Processed {len(results)} items")

            # Show first few results
            for i, result in enumerate(results[:3]):
                print(f"  {i+1}: {result}")

            if len(results) > 3:
                print(f"  ... and {len(results) - 3} more")

        except Exception as e:
            print(f"  Error: {e}")

if __name__ == "__main__":
    interactive_test()
    test_with_sample_files()
```

### Visual Testing
```python
# examples/visual_test.py
"""Visual testing to see what your code does."""

def visual_test_file_organization():
    """Visually test file organization."""
    from pathlib import Path

    test_dir = Path('/tmp/test_organization')
    test_dir.mkdir(exist_ok=True)

    # Create test files
    test_files = [
        'document.pdf',
        'photo.jpg',
        'data.csv',
        'script.py',
        'notes.txt'
    ]

    for filename in test_files:
        (test_dir / filename).touch()

    print("Before organization:")
    for item in test_dir.iterdir():
        print(f"  {item.name}")

    # Run organization
    from core.file_organizer import organize_files
    organize_files(test_dir)

    print("\nAfter organization:")
    for item in test_dir.rglob('*'):
        if item.is_file():
            relative_path = item.relative_to(test_dir)
            print(f"  {relative_path}")

    # Clean up
    import shutil
    shutil.rmtree(test_dir)

if __name__ == "__main__":
    visual_test_file_organization()
```

## Quality Checks (Keep It Simple)

### Basic Code Quality Check
```python
# check_quality.py
"""Simple code quality checker."""

import ast
from pathlib import Path

def check_python_syntax():
    """Check that all Python files have valid syntax."""
    src_dir = Path('/app/app_i_am_developing/src')
    errors = []

    for py_file in src_dir.rglob('*.py'):
        try:
            with open(py_file, 'r') as f:
                content = f.read()

            # Try to parse the file
            ast.parse(content)
            print(f"✓ {py_file.relative_to(src_dir)}")

        except SyntaxError as e:
            error_msg = f"✗ {py_file.relative_to(src_dir)}: {e}"
            print(error_msg)
            errors.append(error_msg)
        except Exception as e:
            error_msg = f"✗ {py_file.relative_to(src_dir)}: {e}"
            print(error_msg)
            errors.append(error_msg)

    return len(errors) == 0

def check_basic_standards():
    """Check basic coding standards."""
    issues = []
    src_dir = Path('/app/app_i_am_developing/src')

    for py_file in src_dir.rglob('*.py'):
        with open(py_file, 'r') as f:
            lines = f.readlines()

        # Check for common issues
        for i, line in enumerate(lines, 1):
            # Check line length (basic check)
            if len(line) > 120:
                issues.append(f"{py_file.name}:{i} - Line too long ({len(line)} chars)")

            # Check for print statements in main code (basic check)
            if 'print(' in line and '__main__' not in line and 'def test' not in line:
                issues.append(f"{py_file.name}:{i} - Consider using logging instead of print")

    if issues:
        print("Code quality issues found:")
        for issue in issues[:10]:  # Show first 10
            print(f"  {issue}")
        if len(issues) > 10:
            print(f"  ... and {len(issues) - 10} more")
    else:
        print("✓ No basic quality issues found")

    return len(issues) == 0

def main():
    """Run all quality checks."""
    print("=== Code Quality Check ===")

    syntax_ok = check_python_syntax()
    standards_ok = check_basic_standards()

    if syntax_ok and standards_ok:
        print("\n🎉 All quality checks passed!")
    else:
        print("\n⚠️  Some issues found - consider fixing them")

if __name__ == "__main__":
    main()
```

## Error Handling Validation

### Test Error Handling
```python
# tests/test_error_handling.py
"""Test that error handling works as expected."""

def test_file_not_found():
    """Test handling of missing files."""
    from core.processor import DataProcessor

    processor = DataProcessor()

    # Test with non-existent file
    result = processor.process_file('does_not_exist.json')

    # Should handle gracefully (return empty list or None)
    assert result is None or result == [], "Should handle missing files gracefully"

    print("✓ File not found handling works")

def test_invalid_data():
    """Test handling of invalid data."""
    from core.processor import DataProcessor

    processor = DataProcessor()

    # Test with invalid data
    invalid_inputs = [
        None,
        [],
        {},
        "not a valid structure"
    ]

    for invalid_input in invalid_inputs:
        try:
            result = processor.process_data(invalid_input)
            # Should not crash, even with invalid input
            print(f"✓ Handled invalid input: {type(invalid_input)}")
        except Exception as e:
            print(f"✗ Failed to handle {type(invalid_input)}: {e}")

def test_edge_cases():
    """Test edge cases that might break the code."""
    from core.processor import DataProcessor

    processor = DataProcessor()

    edge_cases = [
        # Empty structures
        [],
        {},
        "",

        # Very large data
        [{'name': f'item_{i}', 'value': i} for i in range(1000)],

        # Unicode data
        [{'name': 'café', 'value': '中文'}],

        # Mixed types
        [{'name': 123, 'value': 'text'}, {'name': 'text', 'value': 456}]
    ]

    for i, test_case in enumerate(edge_cases):
        try:
            result = processor.process_data(test_case)
            print(f"✓ Edge case {i+1} handled")
        except Exception as e:
            print(f"✗ Edge case {i+1} failed: {e}")

if __name__ == "__main__":
    test_file_not_found()
    test_invalid_data()
    test_edge_cases()
```

## Performance Validation (Basic)

### Simple Performance Check
```python
# tests/test_performance.py
"""Basic performance testing."""

import time
from pathlib import Path

def test_processing_speed():
    """Test that processing doesn't take too long."""
    from core.processor import DataProcessor

    processor = DataProcessor()

    # Create test data
    large_data = [
        {'name': f'item_{i}', 'value': i}
        for i in range(1000)
    ]

    # Time the processing
    start_time = time.time()
    results = processor.process_data(large_data)
    end_time = time.time()

    processing_time = end_time - start_time

    # Basic performance check
    assert processing_time < 5.0, f"Processing took {processing_time:.2f}s, should be under 5s"
    assert len(results) == len(large_data), "Should process all items"

    print(f"✓ Processed {len(large_data)} items in {processing_time:.2f}s")

def test_memory_usage():
    """Basic memory usage check."""
    import psutil
    import os

    process = psutil.Process(os.getpid())
    initial_memory = process.memory_info().rss / 1024 / 1024  # MB

    # Do some processing
    from core.processor import DataProcessor
    processor = DataProcessor()

    # Process large data
    large_data = [{'name': f'item_{i}', 'value': i} for i in range(5000)]
    results = processor.process_data(large_data)

    final_memory = process.memory_info().rss / 1024 / 1024  # MB
    memory_increase = final_memory - initial_memory

    print(f"Memory usage: {initial_memory:.1f}MB -> {final_memory:.1f}MB (+{memory_increase:.1f}MB)")

    # Basic check (adjust threshold as needed)
    assert memory_increase < 100, f"Memory increased by {memory_increase:.1f}MB, should be under 100MB"

    print("✓ Memory usage within reasonable bounds")

if __name__ == "__main__":
    test_processing_speed()
    test_memory_usage()
```

## Continuous Quality (Simple Automation)

### Pre-commit Quality Check
```python
# check_before_commit.py
"""Simple quality check before committing code."""

def check_all():
    """Run all quality checks."""
    import subprocess

    checks = [
        ("Syntax check", "python check_quality.py"),
        ("Basic tests", "python -m app_i_am_developing.tests.test_basic"),
        ("Error handling", "python -m app_i_am_developing.tests.test_error_handling")
    ]

    all_passed = True

    for check_name, command in checks:
        print(f"\n=== {check_name} ===")

        try:
            result = subprocess.run(
                command.split(),
                capture_output=True,
                text=True,
                cwd='/app'
            )

            if result.returncode == 0:
                print(f"✓ {check_name} passed")
            else:
                print(f"✗ {check_name} failed:")
                print(result.stdout)
                print(result.stderr)
                all_passed = False

        except Exception as e:
            print(f"✗ {check_name} error: {e}")
            all_passed = False

    if all_passed:
        print("\n🎉 All checks passed! Ready to commit.")
    else:
        print("\n⚠️  Some checks failed. Fix issues before committing.")

    return all_passed

if __name__ == "__main__":
    success = check_all()
    exit(0 if success else 1)
```

### Docker Quality Check
```bash
#!/bin/bash
# docker_quality_check.sh

echo "Running quality checks in Docker..."

# Start container
docker-compose up -d

# Run quality checks
echo "=== Syntax and Style ==="
docker-compose exec app python check_quality.py

echo "=== Basic Tests ==="
docker-compose exec app python -m app_i_am_developing.tests.test_basic

echo "=== Error Handling Tests ==="
docker-compose exec app python -m app_i_am_developing.tests.test_error_handling

echo "=== Performance Tests ==="
docker-compose exec app python -m app_i_am_developing.tests.test_performance

echo "Quality check complete!"
```

## Learning from Testing

### Test-Driven Learning
```python
def learn_by_testing():
    """Use tests to understand how code should work."""

    # 1. Write a test that describes what you want
    def test_what_i_want():
        result = my_function("test input")
        assert result == "expected output"

    # 2. Run the test (it will fail)
    # 3. Write the minimal code to make it pass
    # 4. Improve the code while keeping the test passing

    pass

def document_with_tests():
    """Use tests as documentation."""

    # Tests show how to use your functions
    def test_usage_examples():
        # Example 1: Basic usage
        result = format_name("john doe")
        assert result == "John Doe"

        # Example 2: Edge case
        result = format_name("")
        assert result == ""

        # Example 3: Invalid input
        result = format_name(None)
        assert result == ""

    # These tests serve as usage examples
    pass
```

## Remember: Keep It Simple

### Quality Principles for Personal Projects
- **Test what matters**: Focus on code that processes your important data
- **Manual testing first**: Run with real data before automating
- **Simple assertions**: Basic checks are better than complex test frameworks
- **Readable tests**: Tests should be easy to understand and modify
- **Fix issues promptly**: Don't let problems accumulate

### Quality Checklist
```python
# Before finishing a feature:
"""
✓ Does it work with real data?
✓ Does it handle missing/invalid files gracefully?
✓ Are error messages helpful?
✓ Is the code easy to read and understand?
✓ Can I run it reliably in Docker?
✓ Did I test the edge cases I care about?
"""
```

The goal is building confidence in your personal tools, not achieving enterprise-level test coverage. Good enough is often good enough!
