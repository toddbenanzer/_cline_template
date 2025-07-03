# Python Development for Personal Applications

## Development Philosophy

### Core Principles
- **Readability over elegance**: Code should be easy to understand and modify
- **Minimal features first**: Start with basic functionality, add complexity later
- **Simple error handling**: Only catch errors you can actually handle
- **Clear over clever**: Straightforward solutions beat complex ones
- **Personal use focus**: Optimize for your needs, not enterprise requirements

## Code Style for Personal Projects

### Function Structure
```python
def process_data(input_data):
    """
    Process input data and return results.

    Keep docstrings simple and focused on what the function does.
    """
    # Step 1: Validate input (minimal)
    if not input_data:
        return None

    # Step 2: Process data
    results = []
    for item in input_data:
        processed_item = transform_item(item)
        results.append(processed_item)

    # Step 3: Return results
    return results

def transform_item(item):
    """Transform a single item - keep functions small and focused."""
    # Simple transformation logic
    return item.upper().strip()
```

### Variable Naming
```python
# Good - clear and descriptive
user_email = "user@example.com"
file_paths = ["doc1.txt", "doc2.txt"]
total_count = len(file_paths)

# Avoid - too short or unclear
e = "user@example.com"
fps = ["doc1.txt", "doc2.txt"]
tc = len(fps)
```

### Error Handling (Minimal Approach)
```python
def read_file_simple(file_path):
    """Read file with minimal error handling."""
    try:
        with open(file_path, 'r') as f:
            return f.read()
    except FileNotFoundError:
        print(f"File {file_path} not found")
        return None
    # Let other errors bubble up - don't hide them
```

## Project Structure for Your Environment

### Application Code Organization
```
app_i_am_developing/
├── src/                           # Main application code
│   ├── __init__.py
│   ├── main.py                    # Entry point
│   ├── core/                      # Core functionality
│   │   ├── __init__.py
│   │   ├── data_processor.py
│   │   └── file_manager.py
│   └── utils/                     # Helper functions
│       ├── __init__.py
│       └── helpers.py
├── examples/                      # Usage examples
│   ├── basic_usage.py
│   └── sample_data.txt
├── tests/                         # Simple tests
│   ├── test_basic.py
│   └── test_data/
├── notebooks/                     # Jupyter notebooks
│   └── exploration.ipynb
├── config/                        # Configuration files
│   ├── settings.py
│   └── config.yml
└── data/                          # Input/output data
    ├── input/
    └── output/
```

### Main Entry Point Pattern
```python
# src/main.py
"""Main entry point for the application."""

from src.core.data_processor import DataProcessor
from src.config.settings import load_config

def main():
    """Main application function."""
    print("Starting application...")

    # Load configuration
    config = load_config()

    # Initialize processor
    processor = DataProcessor(config)

    # Process data
    results = processor.run()

    # Display results
    print(f"Processed {len(results)} items")
    print("Application finished!")

if __name__ == "__main__":
    main()
```

## Docker Development Patterns

### Python Environment Setup
```dockerfile
# Dockerfile
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies (if needed)
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements first (for better caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app_i_am_developing/ ./app_i_am_developing/

# Set Python path
ENV PYTHONPATH=/app

# Default command
CMD ["python", "-m", "app_i_am_developing.src.main"]
```

### Development Docker Compose
```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    volumes:
      - .:/app
      - app_data:/app/app_i_am_developing/data
    working_dir: /app
    stdin_open: true
    tty: true
    environment:
      - PYTHONPATH=/app
      - DEVELOPMENT_MODE=true
    ports:
      - "8888:8888"  # For Jupyter if needed

volumes:
  app_data:
```

## Data Processing Patterns

### File Processing
```python
from pathlib import Path

def process_files_in_folder(folder_path, file_extension="txt"):
    """Process all files of a specific type in a folder."""
    folder = Path(folder_path)

    if not folder.exists():
        print(f"Folder {folder_path} doesn't exist")
        return []

    results = []
    for file_path in folder.glob(f"*.{file_extension}"):
        try:
            result = process_single_file(file_path)
            results.append(result)
            print(f"Processed: {file_path.name}")
        except Exception as e:
            print(f"Error processing {file_path.name}: {e}")

    return results

def process_single_file(file_path):
    """Process a single file."""
    with open(file_path, 'r') as f:
        content = f.read()

    # Simple processing
    lines = content.strip().split('\n')
    return {
        'filename': file_path.name,
        'line_count': len(lines),
        'content': content
    }
```

### Data Transformation
```python
def clean_data(raw_data):
    """Clean and transform raw data."""
    if not raw_data:
        return []

    cleaned_data = []
    for item in raw_data:
        # Simple cleaning
        cleaned_item = {
            'id': item.get('id', ''),
            'name': item.get('name', '').strip().title(),
            'value': item.get('value', 0)
        }

        # Only include valid items
        if cleaned_item['id'] and cleaned_item['name']:
            cleaned_data.append(cleaned_item)

    return cleaned_data
```

## Configuration Management

### Simple Configuration
```python
# config/settings.py
"""Application configuration."""

import os
from pathlib import Path

class Config:
    """Simple configuration class."""

    def __init__(self):
        self.base_dir = Path(__file__).parent.parent
        self.data_dir = self.base_dir / "data"
        self.input_dir = self.data_dir / "input"
        self.output_dir = self.data_dir / "output"

        # Create directories if they don't exist
        self.input_dir.mkdir(parents=True, exist_ok=True)
        self.output_dir.mkdir(parents=True, exist_ok=True)

        # Simple settings
        self.batch_size = 100
        self.timeout = 30
        self.debug = os.getenv('DEBUG', 'false').lower() == 'true'

def load_config():
    """Load configuration."""
    return Config()
```

### Environment Variables
```python
# .env file (in development_folder)
DEBUG=true
DATA_PATH=/app/app_i_am_developing/data
BATCH_SIZE=50
```

```python
# Using environment variables
import os

def get_setting(key, default=None):
    """Get setting from environment or use default."""
    return os.getenv(key, default)

# Usage
debug_mode = get_setting('DEBUG', 'false').lower() == 'true'
data_path = get_setting('DATA_PATH', './data')
batch_size = int(get_setting('BATCH_SIZE', '100'))
```

## Testing (Simple and Practical)

### Basic Testing Pattern
```python
# tests/test_basic.py
"""Basic tests for core functionality."""

import sys
from pathlib import Path

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent / "src"))

from core.data_processor import DataProcessor

def test_data_processor():
    """Test basic data processing functionality."""
    processor = DataProcessor()

    # Test with simple data
    test_data = [
        {'id': '1', 'name': 'test', 'value': 10},
        {'id': '2', 'name': 'example', 'value': 20}
    ]

    result = processor.process(test_data)

    # Simple assertions
    assert len(result) == 2
    assert result[0]['id'] == '1'
    print("✓ Data processor test passed")

def test_file_operations():
    """Test file operations."""
    from core.file_manager import FileManager

    manager = FileManager()

    # Test with temporary file
    test_file = Path("test_temp.txt")
    test_file.write_text("test content")

    try:
        content = manager.read_file(test_file)
        assert content == "test content"
        print("✓ File operations test passed")
    finally:
        test_file.unlink()  # Clean up

def run_all_tests():
    """Run all tests."""
    print("Running tests...")
    test_data_processor()
    test_file_operations()
    print("All tests passed! ✓")

if __name__ == "__main__":
    run_all_tests()
```

## Development Workflow

### Daily Development Process
```python
# Daily workflow checklist
"""
1. Start Docker container: docker-compose up
2. Check memory bank for current goals
3. Pick ONE small feature to implement
4. Write the simplest version that works
5. Test manually with real data
6. Clean up code for readability
7. Update memory bank with progress
8. Commit working code
"""
```

### Code Review Checklist
```python
# Before committing code, check:
"""
✓ Variable names are clear and descriptive
✓ Functions are small and do one thing
✓ No hardcoded values (use config)
✓ Minimal error handling where needed
✓ Code is formatted consistently
✓ Comments explain 'why', not 'what'
✓ Tested manually with real data
"""
```

## Common Patterns for Personal Use

### File Organization
```python
def organize_downloads():
    """Organize downloads folder by file type."""
    from pathlib import Path

    downloads = Path.home() / "Downloads"

    file_types = {
        '.pdf': 'documents',
        '.jpg': 'images',
        '.png': 'images',
        '.txt': 'documents',
        '.py': 'code'
    }

    for file in downloads.iterdir():
        if file.is_file():
            extension = file.suffix.lower()
            folder_name = file_types.get(extension, 'other')

            # Create folder if needed
            dest_folder = downloads / folder_name
            dest_folder.mkdir(exist_ok=True)

            # Move file
            dest_path = dest_folder / file.name
            file.rename(dest_path)
            print(f"Moved {file.name} to {folder_name}/")
```

### Data Processing
```python
def process_csv_data(csv_file):
    """Process CSV data simply."""
    import csv

    results = []
    with open(csv_file, 'r') as f:
        reader = csv.DictReader(f)
        for row in reader:
            # Simple processing
            processed_row = {
                'name': row['name'].strip().title(),
                'value': float(row['value']) if row['value'] else 0
            }
            results.append(processed_row)

    return results
```

## Learning Path

### Focus Areas for Your Skill Level
1. **File operations**: pathlib, reading/writing files
2. **Data processing**: lists, dictionaries, simple transformations
3. **Basic modules**: csv, json, datetime, os
4. **Simple classes**: when and how to use them
5. **Project organization**: modules, imports, structure

### Next Steps
- Practice with pathlib for file operations
- Learn pandas for data processing
- Understand when to use classes vs functions
- Get comfortable with imports and modules
- Build small, complete applications

Remember: Keep it simple, readable, and focused on solving your actual problems!
