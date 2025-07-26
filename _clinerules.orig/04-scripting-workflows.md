# Personal Scripting Workflows and Development Patterns

## Daily Development Routine

### Container Startup Checklist

```bash
# 1. Open VS Code
# 2. Open folder in container (F1 -> "Remote-Containers: Open Folder in Container")
# 3. Wait for container to build/start
# 4. In terminal, verify environment:

cd /app/app_i_am_developing
python --version  # Should show 3.11+
pip list  # Check installed packages
ls -la  # Verify file structure

# 5. Start Cline with confidence check:
"Follow your custom instructions and read all memory bank files.
Rate your confidence (0-10) in understanding the current project state."
```

### Project Structure Setup

```python
# Script: /app/app_i_am_developing/scripts/setup_project.py
"""
Set up standard project structure for new development.

Learning points:
- Path manipulation with pathlib
- Directory creation patterns
- File templates
"""

from pathlib import Path
import json


def setup_project_structure(project_name="my_project"):
    """Create standard directory structure."""
    base_path = Path(f"/app/app_i_am_developing/{project_name}")

    # Define directory structure
    directories = [
        "src",
        "tests",
        "data/input",
        "data/output",
        "config",
        "docs",
        "notebooks",
        "examples"
    ]

    # Create directories
    for dir_name in directories:
        dir_path = base_path / dir_name
        dir_path.mkdir(parents=True, exist_ok=True)
        print(f"✅ Created {dir_path}")

    # Create standard files
    files_to_create = {
        "README.md": f"# {project_name}\n\nProject description here.",
        "requirements.txt": "# Add your dependencies here\npython-dotenv\nrequests\n",
        ".env.example": "# Copy to .env and fill in your values\nAPI_KEY=your_key_here\n",
        ".gitignore": ".env\n__pycache__/\n*.pyc\ndata/output/*\n!data/output/.gitkeep\n",
        "src/__init__.py": "",
        "data/output/.gitkeep": ""
    }

    for file_path, content in files_to_create.items():
        full_path = base_path / file_path
        full_path.parent.mkdir(parents=True, exist_ok=True)
        full_path.write_text(content)
        print(f"✅ Created {full_path}")

    print(f"\n🎉 Project '{project_name}' is ready!")
    print(f"   Location: {base_path}")


if __name__ == "__main__":
    setup_project_structure()
```

## Project Type Workflows

### Web Scraping Workflow

```python
# Workflow stages for building a scraper

# Stage 1: Exploration
"""
Task: Understand the website structure
1. Manually visit the site
2. Inspect HTML (F12 in browser)
3. Identify data patterns
4. Check robots.txt
"""

# Stage 2: Prototype
# File: /app/app_i_am_developing/notebooks/scraper_exploration.ipynb
import requests
from bs4 import BeautifulSoup

# Test single page fetch
url = "https://example.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

# Explore structure
print("Title:", soup.title.text)
print("Links:", len(soup.find_all('a')))

# Stage 3: Build Core Scraper
# File: /app/app_i_am_developing/src/scrapers/main_scraper.py
"""
Production scraper with error handling and logging.

Progressive versions:
- V1: Basic fetch and parse
- V2: Add rate limiting and retries
- V3: Add data validation and storage
- V4: Add progress tracking and resume capability
"""

import time
import json
from pathlib import Path
from datetime import datetime
import logging

# Set up logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)


class ProgressiveScraper:
    """Web scraper that improves with each version."""

    def __init__(self, base_url, version=1):
        self.base_url = base_url
        self.version = version
        self.session = requests.Session()
        self.data_dir = Path("/app/app_i_am_developing/data/output")

    def scrape_v1(self, url):
        """Version 1: Basic scraping."""
        response = self.session.get(url)
        soup = BeautifulSoup(response.text, 'html.parser')
        return {"title": soup.title.text}

    def scrape_v2(self, url):
        """Version 2: Add politeness and error handling."""
        time.sleep(1)  # Be polite

        try:
            response = self.session.get(url, timeout=10)
            response.raise_for_status()
            soup = BeautifulSoup(response.text, 'html.parser')
            return {"title": soup.title.text, "status": "success"}
        except Exception as e:
            logger.error(f"Failed to scrape {url}: {e}")
            return {"error": str(e), "status": "failed"}

    def scrape_v3(self, urls):
        """Version 3: Batch processing with progress."""
        results = []

        for i, url in enumerate(urls, 1):
            logger.info(f"Scraping {i}/{len(urls)}: {url}")
            result = self.scrape_v2(url)
            result['url'] = url
            result['timestamp'] = datetime.now().isoformat()
            results.append(result)

            # Save progress
            if i % 10 == 0:
                self.save_progress(results)

        return results

    def save_progress(self, data):
        """Save scraping progress."""
        filename = f"scrape_results_{datetime.now():%Y%m%d_%H%M%S}.json"
        filepath = self.data_dir / filename

        with open(filepath, 'w') as f:
            json.dump(data, f, indent=2)

        logger.info(f"Saved progress to {filepath}")


# Usage example showing progression
if __name__ == "__main__":
    scraper = ProgressiveScraper("https://example.com")

    # Start simple
    print("Version 1:", scraper.scrape_v1("https://example.com"))

    # Add features gradually
    print("Version 2:", scraper.scrape_v2("https://example.com"))
```

### LLM API Module Workflow

```python
# Workflow for building LLM integration modules

# Stage 1: API Key Management
# File: /app/app_i_am_developing/config/llm_config.py
"""
Centralized LLM configuration.

Learning points:
- Environment variable management
- Configuration validation
- Multiple API support
"""

import os
from dotenv import load_dotenv
from typing import Optional

load_dotenv()


class LLMConfig:
    """Manage LLM API configurations."""

    def __init__(self):
        self.gemini_key = os.getenv('GEMINI_API_KEY')
        self.openai_key = os.getenv('OPENAI_API_KEY')

    def get_available_models(self):
        """Check which models we can use."""
        available = []

        if self.gemini_key:
            available.append("gemini")
        if self.openai_key:
            available.append("openai")

        return available

    def validate(self):
        """Ensure at least one API is configured."""
        if not self.gemini_key and not self.openai_key:
            raise ValueError(
                "No API keys found! Add GEMINI_API_KEY or "
                "OPENAI_API_KEY to your .env file"
            )


# Stage 2: Simple LLM Interface
# File: /app/app_i_am_developing/src/llm/simple_llm.py
"""
Simple interface for multiple LLMs.

Progressive versions:
- V1: Basic prompt/response
- V2: Add conversation memory
- V3: Add prompt templates
- V4: Add cost tracking
"""

from typing import List, Dict, Optional
import google.generativeai as genai
from config.llm_config import LLMConfig


class SimpleLLM:
    """Simple multi-LLM interface."""

    def __init__(self, model_type="gemini"):
        self.config = LLMConfig()
        self.config.validate()
        self.model_type = model_type
        self.conversation_history = []

        # Initialize model
        if model_type == "gemini":
            genai.configure(api_key=self.config.gemini_key)
            self.model = genai.GenerativeModel('gemini-2.0-flash-experimental')

    def ask(self, prompt: str, remember: bool = True) -> str:
        """Send prompt to LLM."""
        try:
            # Add to history if remembering
            if remember:
                self.conversation_history.append({
                    "role": "user",
                    "content": prompt
                })

            # Get response
            response = self.model.generate_content(prompt)

            # Save response to history
            if remember:
                self.conversation_history.append({
                    "role": "assistant",
                    "content": response.text
                })

            return response.text

        except Exception as e:
            return f"Error: {e}"

    def ask_with_context(self, prompt: str, context: str) -> str:
        """Ask with additional context."""
        full_prompt = f"""
        Context: {context}

        Question: {prompt}

        Please answer based on the provided context.
        """
        return self.ask(full_prompt)

    def summarize_text(self, text: str, max_length: int = 100) -> str:
        """Summarize long text."""
        prompt = f"""
        Please summarize this text in about {max_length} words:

        {text}

        Summary:
        """
        return self.ask(prompt, remember=False)


# Stage 3: Specialized Modules
# File: /app/app_i_am_developing/src/llm/code_assistant.py
"""
LLM module specialized for code assistance.
"""

class CodeAssistant(SimpleLLM):
    """LLM assistant for coding tasks."""

    def explain_error(self, error_message: str, code_context: str = "") -> str:
        """Explain Python errors in simple terms."""
        prompt = f"""
        I got this Python error:
        {error_message}

        Code context:
        {code_context}

        Please explain:
        1. What this error means in simple terms
        2. Why it happened
        3. How to fix it
        4. How to avoid it in the future

        Keep explanation beginner-friendly (skill level 4/10).
        """
        return self.ask(prompt)

    def improve_code(self, code: str, focus: str = "readability") -> str:
        """Get code improvement suggestions."""
        prompt = f"""
        Please improve this Python code focusing on {focus}:

        {code}

        Requirements:
        - Keep it simple (I'm skill level 4/10)
        - Preserve the core logic
        - Add helpful comments
        - Explain each improvement

        Provide the improved code and explanations.
        """
        return self.ask(prompt)
```

### Data Science Workflow

```python
# Progressive data science project workflow

# Stage 1: Data Exploration
# File: /app/app_i_am_developing/notebooks/data_exploration.py
"""
Initial data exploration script.

Learning path:
1. Load and inspect data
2. Basic statistics
3. Simple visualizations
4. Initial findings
"""

import pandas as pd
import matplotlib.pyplot as plt
from pathlib import Path

# Set up paths
data_dir = Path("/app/app_i_am_developing/data")
input_file = data_dir / "input" / "sample_data.csv"


def explore_data(filepath):
    """Basic data exploration routine."""
    print(f"Loading data from {filepath}")

    # Load data
    df = pd.read_csv(filepath)

    # Basic info
    print("\n=== Data Shape ===")
    print(f"Rows: {len(df)}")
    print(f"Columns: {len(df.columns)}")

    print("\n=== Column Types ===")
    print(df.dtypes)

    print("\n=== First 5 Rows ===")
    print(df.head())

    print("\n=== Basic Statistics ===")
    print(df.describe())

    print("\n=== Missing Values ===")
    print(df.isnull().sum())

    return df


# Stage 2: Data Cleaning
# File: /app/app_i_am_developing/src/data/cleaning_pipeline.py
"""
Progressive data cleaning pipeline.
"""

class DataCleaner:
    """Clean data step by step."""

    def __init__(self, df):
        self.original_df = df
        self.df = df.copy()
        self.cleaning_log = []

    def remove_duplicates(self):
        """Remove duplicate rows."""
        before = len(self.df)
        self.df = self.df.drop_duplicates()
        after = len(self.df)

        self.cleaning_log.append(
            f"Removed {before - after} duplicate rows"
        )
        return self

    def handle_missing(self, strategy="drop"):
        """Handle missing values."""
        if strategy == "drop":
            before = len(self.df)
            self.df = self.df.dropna()
            after = len(self.df)
            self.cleaning_log.append(
                f"Dropped {before - after} rows with missing values"
            )
        elif strategy == "fill":
            # Fill numeric columns with median
            numeric_cols = self.df.select_dtypes(include=['number']).columns
            for col in numeric_cols:
                median_val = self.df[col].median()
                self.df[col].fillna(median_val, inplace=True)
            self.cleaning_log.append("Filled missing values with median")

        return self

    def get_clean_data(self):
        """Get cleaned dataframe with report."""
        print("=== Cleaning Report ===")
        for step in self.cleaning_log:
            print(f"- {step}")

        return self.df


# Stage 3: Simple Analysis
# File: /app/app_i_am_developing/src/analysis/basic_analysis.py
"""
Basic analysis patterns for learning.
"""

def analyze_correlations(df, target_column):
    """Find correlations with target variable."""
    # Only numeric columns
    numeric_df = df.select_dtypes(include=['number'])

    # Calculate correlations
    correlations = numeric_df.corr()[target_column].sort_values(
        ascending=False
    )

    print(f"=== Correlations with {target_column} ===")
    for col, corr in correlations.items():
        if col != target_column:
            print(f"{col}: {corr:.3f}")

    return correlations


def create_simple_plots(df, columns_to_plot):
    """Create basic visualizations."""
    fig, axes = plt.subplots(2, 2, figsize=(10, 8))
    axes = axes.ravel()

    for i, col in enumerate(columns_to_plot[:4]):
        if col in df.columns:
            df[col].hist(ax=axes[i], bins=20)
            axes[i].set_title(f'Distribution of {col}')
            axes[i].set_xlabel(col)
            axes[i].set_ylabel('Frequency')

    plt.tight_layout()
    plt.savefig('/app/app_i_am_developing/data/output/distributions.png')
    print("✅ Saved plot to data/output/distributions.png")
```

### Automation Script Workflow

```python
# Building personal automation tools

# Stage 1: Identify Repetitive Task
# File: /app/app_i_am_developing/scripts/file_organizer.py
"""
Organize downloads folder by file type.

Learning goals:
- File system operations
- Pattern matching
- Automation basics
"""

from pathlib import Path
import shutil
from datetime import datetime


class FileOrganizer:
    """Organize files by type and date."""

    def __init__(self, source_dir, target_dir):
        self.source_dir = Path(source_dir)
        self.target_dir = Path(target_dir)

        # File type mappings
        self.file_mappings = {
            'images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp'],
            'documents': ['.pdf', '.doc', '.docx', '.txt', '.odt'],
            'spreadsheets': ['.xls', '.xlsx', '.csv'],
            'archives': ['.zip', '.rar', '.7z', '.tar', '.gz'],
            'code': ['.py', '.js', '.html', '.css', '.json'],
            'data': ['.csv', '.json', '.xml', '.sql']
        }

    def organize_by_type(self, dry_run=True):
        """Organize files into type folders."""
        moves_planned = []

        # Check all files
        for file_path in self.source_dir.iterdir():
            if file_path.is_file():
                file_type = self.get_file_type(file_path)

                if file_type:
                    # Create target directory
                    type_dir = self.target_dir / file_type
                    type_dir.mkdir(exist_ok=True)

                    # Plan move
                    target_path = type_dir / file_path.name
                    moves_planned.append((file_path, target_path))

        # Execute or preview
        if dry_run:
            print("=== Planned Moves (Dry Run) ===")
            for source, target in moves_planned:
                print(f"Move: {source.name} -> {target.parent.name}/")
        else:
            for source, target in moves_planned:
                shutil.move(str(source), str(target))
                print(f"✅ Moved {source.name}")

        return len(moves_planned)

    def get_file_type(self, file_path):
        """Determine file type category."""
        suffix = file_path.suffix.lower()

        for file_type, extensions in self.file_mappings.items():
            if suffix in extensions:
                return file_type

        return None


# Stage 2: Schedule Automation
# File: /app/app_i_am_developing/scripts/daily_tasks.py
"""
Daily automation tasks.
"""

def run_daily_tasks():
    """Run all daily automation tasks."""
    print(f"=== Daily Tasks - {datetime.now():%Y-%m-%d %H:%M} ===")

    # Task 1: Organize downloads
    organizer = FileOrganizer(
        "/app/app_i_am_developing/data/input",
        "/app/app_i_am_developing/data/organized"
    )
    count = organizer.organize_by_type(dry_run=False)
    print(f"✅ Organized {count} files")

    # Task 2: Clean old files
    clean_old_files("/app/app_i_am_developing/data/temp", days=7)

    # Task 3: Backup important data
    backup_data("/app/app_i_am_developing/data/important")

    print("=== Daily tasks complete ===")
```

## Development Patterns

### Progressive Enhancement Pattern

```python
def progressive_development():
    """
    Build features incrementally:

    Day 1: Make it work
    Day 2: Make it safe
    Day 3: Make it flexible
    Day 4: Make it maintainable
    """
    pass

# Example: Building a web scraper progressively

# Day 1: Just get the data
response = requests.get(url)
data = response.text

# Day 2: Add error handling
try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    data = response.text
except requests.RequestException as e:
    print(f"Error: {e}")
    data = None

# Day 3: Add configuration
def fetch_data(url, timeout=10, retries=3, headers=None):
    """Configurable data fetching."""
    # Implementation

# Day 4: Add logging and monitoring
logger.info(f"Fetching {url}")
# Full implementation with metrics
```

### Error-First Development

```python
# Start by thinking about what can go wrong

def safe_file_operation(filepath):
    """
    File operation with comprehensive error handling.

    What can go wrong:
    1. File doesn't exist
    2. No read permissions
    3. File is corrupted
    4. Disk is full
    5. Network drive disconnected
    """
    filepath = Path(filepath)

    # Check file exists
    if not filepath.exists():
        logger.error(f"File not found: {filepath}")
        return None

    # Check permissions
    if not filepath.is_file():
        logger.error(f"Not a file: {filepath}")
        return None

    try:
        # Actual operation
        with open(filepath, 'r') as f:
            return f.read()
    except PermissionError:
        logger.error(f"No permission to read: {filepath}")
    except IOError as e:
        logger.error(f"IO error reading {filepath}: {e}")
    except Exception as e:
        logger.error(f"Unexpected error: {e}")

    return None
```

### Testing Patterns for Personal Projects

```python
# Simple testing without complex frameworks

# Pattern 1: Test functions with examples
def test_scraper():
    """Test scraper with real example."""
    scraper = WebScraper("https://example.com")

    # Test 1: Can fetch page
    html = scraper.fetch_page("/test")
    assert html is not None, "Failed to fetch page"

    # Test 2: Can parse data
    data = scraper.parse_data(html)
    assert len(data) > 0, "No data parsed"

    print("✅ All scraper tests passed!")


# Pattern 2: Visual testing for data science
def test_data_pipeline():
    """Test data pipeline with visualization."""
    # Load test data
    df = pd.read_csv("/app/app_i_am_developing/data/test_data.csv")

    # Process data
    cleaned_df = DataCleaner(df).clean()

    # Visual check
    plt.figure(figsize=(10, 6))
    plt.subplot(1, 2, 1)
    df['value'].hist()
    plt.title('Original Data')

    plt.subplot(1, 2, 2)
    cleaned_df['value'].hist()
    plt.title('Cleaned Data')

    plt.savefig('/app/app_i_am_developing/tests/cleaning_comparison.png')
    print("✅ Check tests/cleaning_comparison.png for results")
```

## Memory Bank Integration

### Update Patterns

```markdown
## After Each Session Update:

### activeContext.md
- Current feature: [what you built]
- Next steps: [what's next]
- Blockers: [any issues]
- Confidence: [X/10]

### progress.md
- Completed: [feature name]
- Learned: [Python concepts]
- Code location: [/app/path/to/file.py]
- Works: [Yes/No]

### devnotes.md
- New package: [package==version]
- Config change: [what changed]
- Debug solution: [problem -> solution]
```

## Quick Command Reference

### Development Commands

```bash
# Project setup
python scripts/setup_project.py

# Run scraper
python src/scrapers/main_scraper.py --url "https://example.com"

# Process data
python src/data/process_csv.py --input data.csv

# Start web app
python src/web/app.py

# Run automation
python scripts/daily_tasks.py

# Quick test
python -m pytest tests/ -v

# Interactive testing
python -i src/module.py
```

### Git Workflow for Learning

```bash
# Save progress frequently
git add .
git commit -m "Add: basic scraper function"

# Create learning branches
git checkout -b feature/add-error-handling

# Commit with learning notes
git commit -m "Learn: try/except patterns for API calls"
```

Remember: Every script is a learning opportunity. Start simple, enhance gradually, and document what you learn!
