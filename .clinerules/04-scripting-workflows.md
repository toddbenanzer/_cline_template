# Personal Python Development Workflows

## Development Environment Workflow

### Daily Development Routine
```bash
# 1. Start your development environment
cd /path/to/development_folder
docker-compose up -d

# 2. Open VS Code in WSL2
code .

# 3. Attach to Docker container
# (Use VS Code Dev Containers extension)

# 4. Start Cline session with memory bank
# Use: "follow your custom instructions"

# 5. Check current status from memory bank
python -m app_i_am_developing.src.main --help

# 6. Work on current feature

# 7. End session with memory bank update
# Use: "update memory bank" with today's progress
```

## Memory Bank Integration Workflow

### Session Start Protocol
1. **Read Memory Bank**: Start with "follow your custom instructions"
2. **Verify Context**: Confirm understanding of current project state
3. **Set Session Goals**: Define what to accomplish today
4. **Check Environment**: Ensure Docker container and VS Code are ready

### During Development
- **Document Decisions**: Note why you chose specific approaches
- **Track Learning**: Record new Python concepts as you encounter them
- **Note Problems**: Save error messages and solutions
- **Update Context**: Keep activeContext.md current with progress

### Session End Protocol
```python
# Memory bank update checklist:
"""
□ What specific functions/files did I work on?
□ What Python concepts did I learn or practice?
□ What problems did I solve and how?
□ What's the next immediate step?
□ Any Docker/environment issues encountered?
□ What questions do I have for next session?
"""
```

### Memory Bank Health Indicators
- **Good**: activeContext.md reflects current work accurately
- **Good**: Can resume work quickly using memory bank context
- **Warning**: Memory bank info doesn't match actual project state
- **Bad**: Spending time figuring out what you were working on

### Project Structure Workflow
```
development_folder/
├── docker-compose.yml      # Start here for new projects
├── Dockerfile             # Simple Python environment
├── requirements.txt       # Keep dependencies minimal
├── .env                   # Environment variables
├── .gitignore            # Standard Python gitignore
└── app_i_am_developing/
    ├── src/              # Main application code
    ├── examples/         # Usage examples
    ├── tests/            # Simple tests
    ├── config/           # Configuration files
    └── data/             # Input/output data
```

## Development Patterns for Personal Use

### The "Build One Feature" Pattern
```python
# Day 1: Get basic functionality working
def process_data_v1(data):
    """Version 1: Just make it work."""
    results = []
    for item in data:
        results.append(item.upper())
    return results

# Day 2: Add basic error handling
def process_data_v2(data):
    """Version 2: Add minimal error handling."""
    if not data:
        return []

    results = []
    for item in data:
        if isinstance(item, str):
            results.append(item.upper())
    return results

# Day 3: Make it more useful
def process_data_v3(data, operation='upper'):
    """Version 3: Add configurability."""
    if not data:
        return []

    operations = {
        'upper': str.upper,
        'lower': str.lower,
        'title': str.title
    }

    op_func = operations.get(operation, str.upper)
    results = []

    for item in data:
        if isinstance(item, str):
            results.append(op_func(item))

    return results
```

### The "Personal Tool" Pattern
```python
# Example: Personal file organizer
from pathlib import Path
import shutil

def organize_my_files():
    """Organize files the way I like them."""

    # My specific folder structure
    downloads = Path.home() / "Downloads"
    organized = downloads / "organized"

    # Create folders for my needs
    folders = {
        'documents': organized / "documents",
        'images': organized / "images",
        'code': organized / "code",
        'data': organized / "data"
    }

    # Create folders if they don't exist
    for folder in folders.values():
        folder.mkdir(parents=True, exist_ok=True)

    # My file type mapping
    file_types = {
        '.pdf': 'documents',
        '.txt': 'documents',
        '.py': 'code',
        '.csv': 'data',
        '.json': 'data',
        '.jpg': 'images',
        '.png': 'images'
    }

    # Process files
    for file_path in downloads.iterdir():
        if file_path.is_file():
            extension = file_path.suffix.lower()
            folder_type = file_types.get(extension, 'other')

            if folder_type != 'other':
                dest_folder = folders[folder_type]
                dest_path = dest_folder / file_path.name
                shutil.move(str(file_path), str(dest_path))
                print(f"Moved {file_path.name} to {folder_type}/")

if __name__ == "__main__":
    organize_my_files()
```

## Docker Development Workflows

### Container-Based Development
```yaml
# docker-compose.yml for development
version: '3.8'
services:
  app:
    build: .
    volumes:
      - .:/app                          # Mount entire project
      - app_data:/app/app_i_am_developing/data  # Persist data
    working_dir: /app
    stdin_open: true
    tty: true
    environment:
      - PYTHONPATH=/app
      - DEVELOPMENT_MODE=true
    ports:
      - "8000:8000"                     # For web apps
      - "8888:8888"                     # For Jupyter

volumes:
  app_data:
```

### Development Commands
```bash
# Common development commands
docker-compose exec app python -m app_i_am_developing.src.main
docker-compose exec app python -m app_i_am_developing.tests.test_basic
docker-compose exec app python -c "from app_i_am_developing.src.core import *; help(DataProcessor)"
```

### File Operations in Docker
```python
# Handle file paths in Docker environment
from pathlib import Path
import os

def get_app_paths():
    """Get standard paths for the application."""
    if os.getenv('DEVELOPMENT_MODE'):
        base_path = Path('/app/app_i_am_developing')
    else:
        base_path = Path(__file__).parent.parent

    return {
        'data': base_path / 'data',
        'config': base_path / 'config',
        'examples': base_path / 'examples',
        'output': base_path / 'data' / 'output'
    }

def process_files_in_docker():
    """Example of processing files in Docker."""
    paths = get_app_paths()

    # Ensure directories exist
    paths['output'].mkdir(parents=True, exist_ok=True)

    # Process files
    for input_file in paths['data'].glob('*.txt'):
        output_file = paths['output'] / f"processed_{input_file.name}"

        # Simple processing
        with open(input_file, 'r') as f:
            content = f.read()

        processed_content = content.upper()  # Simple transformation

        with open(output_file, 'w') as f:
            f.write(processed_content)

        print(f"Processed {input_file.name} -> {output_file.name}")
```

## Personal Application Templates

### Simple CLI Application
```python
# src/main.py
"""Simple CLI application template."""

import argparse
from pathlib import Path
from .core.processor import DataProcessor
from .config.settings import load_config

def main():
    """Main application entry point."""
    parser = argparse.ArgumentParser(description='My personal tool')
    parser.add_argument('--input', help='Input file path')
    parser.add_argument('--output', help='Output file path')
    parser.add_argument('--config', help='Config file path')

    args = parser.parse_args()

    # Load configuration
    config = load_config(args.config)

    # Initialize processor
    processor = DataProcessor(config)

    # Process data
    if args.input:
        results = processor.process_file(args.input)

        if args.output:
            processor.save_results(results, args.output)
        else:
            print(f"Processed {len(results)} items")
    else:
        print("Please provide an input file")

if __name__ == "__main__":
    main()
```

### Data Processing Application
```python
# src/core/processor.py
"""Simple data processing core."""

import json
from pathlib import Path

class DataProcessor:
    """Simple data processor for personal use."""

    def __init__(self, config=None):
        self.config = config or {}
        self.batch_size = self.config.get('batch_size', 100)

    def process_file(self, file_path):
        """Process a single file."""
        file_path = Path(file_path)

        if file_path.suffix == '.json':
            return self._process_json(file_path)
        elif file_path.suffix == '.txt':
            return self._process_text(file_path)
        else:
            print(f"Unsupported file type: {file_path.suffix}")
            return []

    def _process_json(self, file_path):
        """Process JSON file."""
        with open(file_path, 'r') as f:
            data = json.load(f)

        # Simple processing
        results = []
        for item in data:
            processed_item = self._process_item(item)
            results.append(processed_item)

        return results

    def _process_text(self, file_path):
        """Process text file."""
        with open(file_path, 'r') as f:
            lines = f.readlines()

        # Simple processing
        results = []
        for line in lines:
            processed_line = line.strip().upper()
            if processed_line:  # Skip empty lines
                results.append(processed_line)

        return results

    def _process_item(self, item):
        """Process a single item."""
        if isinstance(item, dict):
            return {k: str(v).upper() for k, v in item.items()}
        else:
            return str(item).upper()

    def save_results(self, results, output_path):
        """Save results to file."""
        output_path = Path(output_path)

        if output_path.suffix == '.json':
            with open(output_path, 'w') as f:
                json.dump(results, f, indent=2)
        else:
            with open(output_path, 'w') as f:
                for result in results:
                    f.write(f"{result}\n")

        print(f"Saved {len(results)} results to {output_path}")
```

## Testing and Validation Workflows

### Simple Manual Testing
```python
# examples/manual_test.py
"""Manual testing script."""

from pathlib import Path
import sys

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent / 'src'))

from core.processor import DataProcessor

def test_basic_functionality():
    """Test basic functionality manually."""
    processor = DataProcessor()

    # Test with simple data
    test_data = [
        {'name': 'test', 'value': 123},
        {'name': 'example', 'value': 456}
    ]

    # Create temporary JSON file
    test_file = Path('test_data.json')
    import json
    with open(test_file, 'w') as f:
        json.dump(test_data, f)

    try:
        # Process the file
        results = processor.process_file(test_file)

        # Check results
        print(f"Processed {len(results)} items")
        for result in results:
            print(f"  {result}")

        # Save results
        processor.save_results(results, 'test_output.json')

    finally:
        # Clean up
        test_file.unlink()
        Path('test_output.json').unlink()

if __name__ == "__main__":
    test_basic_functionality()
```

### Docker Testing
```bash
# Test script for Docker environment
#!/bin/bash
echo "Testing application in Docker..."

# Start services
docker-compose up -d

# Wait for services to be ready
sleep 5

# Run tests
docker-compose exec app python -m app_i_am_developing.examples.manual_test

# Run main application
docker-compose exec app python -m app_i_am_developing.src.main --help

# Clean up
docker-compose down

echo "Testing complete!"
```

## Configuration and Environment Management

### Simple Environment Configuration
```python
# config/settings.py
"""Simple configuration management."""

import os
from pathlib import Path

class Settings:
    """Application settings."""

    def __init__(self):
        self.debug = os.getenv('DEBUG', 'false').lower() == 'true'
        self.data_path = Path(os.getenv('DATA_PATH', './data'))
        self.batch_size = int(os.getenv('BATCH_SIZE', '100'))
        self.timeout = int(os.getenv('TIMEOUT', '30'))

        # Ensure data directory exists
        self.data_path.mkdir(parents=True, exist_ok=True)

    def get_input_path(self):
        """Get input data path."""
        return self.data_path / 'input'

    def get_output_path(self):
        """Get output data path."""
        return self.data_path / 'output'

def load_config(config_file=None):
    """Load configuration."""
    if config_file:
        # Load from file if specified
        pass  # Add file loading logic if needed

    return Settings()
```

### Environment Variables
```bash
# .env file for development
DEBUG=true
DATA_PATH=/app/app_i_am_developing/data
BATCH_SIZE=50
TIMEOUT=60
DEVELOPMENT_MODE=true
```

## Common Personal Use Cases

### File Management Tasks
```python
def organize_project_files():
    """Organize project files by type."""
    project_root = Path('/app/app_i_am_developing')

    # Define organization rules
    rules = {
        'notebooks': project_root / 'notebooks',
        'examples': project_root / 'examples',
        'data': project_root / 'data',
        'config': project_root / 'config'
    }

    # Process files
    for file_path in project_root.rglob('*'):
        if file_path.is_file():
            file_type = determine_file_type(file_path)
            if file_type in rules:
                move_to_appropriate_folder(file_path, rules[file_type])

def determine_file_type(file_path):
    """Determine file type based on extension and content."""
    if file_path.suffix == '.ipynb':
        return 'notebooks'
    elif file_path.suffix in ['.csv', '.json', '.txt']:
        return 'data'
    elif file_path.name.startswith('example_'):
        return 'examples'
    elif file_path.suffix in ['.yml', '.yaml', '.conf']:
        return 'config'
    return 'other'
```

### Data Processing Tasks
```python
def process_personal_data():
    """Process personal data files."""
    data_dir = Path('/app/app_i_am_developing/data/input')
    output_dir = Path('/app/app_i_am_developing/data/output')

    output_dir.mkdir(parents=True, exist_ok=True)

    for data_file in data_dir.glob('*.csv'):
        processed_data = process_csv_file(data_file)
        output_file = output_dir / f"processed_{data_file.name}"
        save_processed_data(processed_data, output_file)
        print(f"Processed {data_file.name} -> {output_file.name}")

def process_csv_file(file_path):
    """Process CSV file simply."""
    import csv

    results = []
    with open(file_path, 'r') as f:
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

## Development Best Practices

### Keep It Simple Workflow
1. **One feature at a time**: Focus on single functionality
2. **Test manually first**: Run with real data before automating
3. **Use simple patterns**: Stick to basic Python constructs
4. **Document for future self**: Add comments about why, not what
5. **Commit working code**: Save progress frequently

### Personal Development Checklist
```python
# Before finishing a coding session:
"""
✓ Does the code work with real data?
✓ Are variable names clear and descriptive?
✓ Is the logic easy to follow?
✓ Did I add necessary comments?
✓ Can I run this in my Docker environment?
✓ Is it simple enough to maintain?
✓ Did I update the memory bank?
"""
```

Remember: Focus on building tools that solve your actual problems, using simple patterns you can understand and maintain!
