# Python Development Standards for Personal Projects

## Core Philosophy: Readability First

Your code should be like a well-written tutorial - anyone (including future you) should understand it immediately. We prioritize clarity over cleverness, learning over performance optimization.

## Code Structure Standards

### File Organization Pattern

```python
"""
module_name.py - Brief description of what this module does

What you'll learn from this module:
- Concept 1 (e.g., working with APIs)
- Concept 2 (e.g., error handling)
- Concept 3 (e.g., data validation)

Usage example:
    from module_name import main_function
    result = main_function("input data")
    print(result)
"""

# Standard imports first
import os
import sys
from pathlib import Path

# Third-party imports second
import requests
import pandas as pd

# Local imports last
from config import settings
from utils import helpers


# Constants at module level
PROJECT_ROOT = Path("/app/app_i_am_developing")
DATA_DIR = PROJECT_ROOT / "data"
MAX_RETRIES = 3


def main_function(input_data):
    """
    One-line summary of what this function does.

    Detailed explanation of how it works, written like you're
    explaining to a friend who's learning Python.

    Args:
        input_data (str): Description of the input

    Returns:
        dict: Description of what's returned

    Example:
        >>> result = main_function("test")
        >>> print(result)
        {'status': 'success', 'data': 'test processed'}
    """
    # Implementation here
    pass
```

### Naming Conventions for Clarity

```python
# Good variable names - descriptive and clear
user_email = "test@example.com"
is_valid_email = True
retry_count = 0
api_response_data = {}

# Bad variable names - too short or unclear
e = "test@example.com"  # What's 'e'?
flag = True  # What kind of flag?
x = 0  # What's being counted?
data = {}  # What kind of data?

# Function names should describe actions
def fetch_user_data(user_id):  # Good - clear action
    pass

def process(x):  # Bad - process what?
    pass

# Class names should be nouns
class WebScraper:  # Good - clear purpose
    pass

class Process:  # Bad - too vague
    pass
```

## Project-Specific Patterns

### Web Scraping Pattern

```python
# File: /app/app_i_am_developing/src/scrapers/simple_scraper.py

import time
import requests
from bs4 import BeautifulSoup
from pathlib import Path


class SimpleScraper:
    """
    A beginner-friendly web scraper.

    What this teaches:
    - Class structure basics
    - HTTP requests
    - HTML parsing
    - Error handling
    - Data saving
    """

    def __init__(self, base_url, delay=1):
        """
        Initialize scraper with URL and polite delay.

        Args:
            base_url (str): Website to scrape
            delay (int): Seconds to wait between requests (be polite!)
        """
        self.base_url = base_url
        self.delay = delay
        self.session = requests.Session()
        # Add headers to look like a real browser
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Personal Learning Project)'
        })

    def fetch_page(self, url):
        """
        Fetch a single page with error handling.

        Learning points:
        - Try/except blocks catch errors
        - sleep() prevents overwhelming the server
        - raise_for_status() checks for HTTP errors
        """
        try:
            # Be polite - wait before requesting
            time.sleep(self.delay)

            # Make the request
            response = self.session.get(url)

            # Check if request succeeded (200 OK)
            response.raise_for_status()

            return response.text

        except requests.RequestException as e:
            # Log error in a beginner-friendly way
            print(f"❌ Couldn't fetch {url}")
            print(f"   Error: {e}")
            print(f"   Tip: Check if the URL is correct and you have internet")
            return None

    def parse_content(self, html):
        """
        Extract data from HTML.

        Override this method for specific sites.
        """
        if not html:
            return None

        soup = BeautifulSoup(html, 'html.parser')

        # Example: extract all links
        links = []
        for link in soup.find_all('a', href=True):
            links.append({
                'text': link.get_text(strip=True),
                'url': link['href']
            })

        return links

    def save_results(self, data, filename):
        """
        Save scraped data to file.

        Learning points:
        - Path handling for file operations
        - JSON for structured data storage
        - Creating directories if needed
        """
        import json

        # Ensure output directory exists
        output_dir = Path("/app/app_i_am_developing/data/output")
        output_dir.mkdir(parents=True, exist_ok=True)

        # Save as JSON for easy reading
        output_file = output_dir / filename

        try:
            with open(output_file, 'w', encoding='utf-8') as f:
                json.dump(data, f, indent=2, ensure_ascii=False)

            print(f"✅ Saved results to {output_file}")

        except Exception as e:
            print(f"❌ Couldn't save results: {e}")


# Example usage
if __name__ == "__main__":
    # Create scraper instance
    scraper = SimpleScraper("https://example.com")

    # Fetch and parse a page
    html = scraper.fetch_page("https://example.com/page")
    data = scraper.parse_content(html)

    # Save results
    if data:
        scraper.save_results(data, "scraped_links.json")
```

### LLM API Integration Pattern

```python
# File: /app/app_i_am_developing/src/llm/llm_helper.py

import os
from typing import Optional, List, Dict
import google.generativeai as genai
from dotenv import load_dotenv


class LLMHelper:
    """
    Simple helper for LLM API calls.

    What this teaches:
    - Environment variable usage
    - API key management
    - Response parsing
    - Cost awareness
    """

    def __init__(self, model_name="gemini-2.0-flash-experimental"):
        """Initialize LLM with configuration."""
        # Load environment variables from .env file
        load_dotenv()

        # Get API key safely
        self.api_key = os.getenv('GEMINI_API_KEY')
        if not self.api_key:
            raise ValueError(
                "No API key found! "
                "Please add GEMINI_API_KEY to your .env file"
            )

        # Configure the model
        genai.configure(api_key=self.api_key)
        self.model = genai.GenerativeModel(model_name)

        # Track usage for cost awareness
        self.total_prompts = 0
        self.total_tokens = 0  # Approximate

    def ask(self, prompt: str, temperature: float = 0.7) -> Optional[str]:
        """
        Send a prompt to the LLM and get response.

        Args:
            prompt: Your question or instruction
            temperature: Creativity level (0=focused, 1=creative)

        Returns:
            Response text or None if error

        Learning points:
        - API calls can fail - always handle errors
        - Temperature affects response style
        - Track usage to avoid surprises
        """
        try:
            # Track usage
            self.total_prompts += 1

            # Make API call
            response = self.model.generate_content(
                prompt,
                generation_config={
                    'temperature': temperature,
                    'max_output_tokens': 2048,
                }
            )

            # Estimate tokens (rough calculation)
            self.total_tokens += len(prompt.split()) * 1.3
            self.total_tokens += len(response.text.split()) * 1.3

            return response.text

        except Exception as e:
            print(f"❌ LLM Error: {e}")
            print(f"   Tip: Check your API key and internet connection")
            return None

    def ask_with_examples(self, task: str, examples: List[Dict]) -> Optional[str]:
        """
        Ask LLM with few-shot examples.

        Learning points:
        - Few-shot prompting improves results
        - Structured prompts work better
        - Examples guide the model
        """
        # Build prompt with examples
        prompt = f"Task: {task}\n\n"
        prompt += "Here are some examples:\n\n"

        for i, example in enumerate(examples, 1):
            prompt += f"Example {i}:\n"
            prompt += f"Input: {example['input']}\n"
            prompt += f"Output: {example['output']}\n\n"

        prompt += "Now, please complete this task:\n"

        return self.ask(prompt)

    def get_usage_stats(self):
        """Show how much you've used the API."""
        # Rough cost estimate (check current pricing!)
        estimated_cost = (self.total_tokens / 1000) * 0.001

        return {
            'total_prompts': self.total_prompts,
            'estimated_tokens': int(self.total_tokens),
            'estimated_cost_usd': round(estimated_cost, 4)
        }


# Example usage showing good practices
if __name__ == "__main__":
    # Initialize helper
    llm = LLMHelper()

    # Simple question
    response = llm.ask("Explain Python lists in simple terms")
    print(response)

    # Check usage
    stats = llm.get_usage_stats()
    print(f"\n📊 Usage: {stats['total_prompts']} prompts, "
          f"~${stats['estimated_cost_usd']} estimated cost")
```

### Data Processing Pattern

```python
# File: /app/app_i_am_developing/src/data/data_processor.py

import pandas as pd
from pathlib import Path
from typing import Optional


class SimpleDataProcessor:
    """
    Basic data processing for CSV files.

    What this teaches:
    - Pandas basics
    - File I/O patterns
    - Data validation
    - Error handling
    """

    def __init__(self, data_dir: str = "/app/app_i_am_developing/data"):
        """Initialize with data directory."""
        self.data_dir = Path(data_dir)
        self.input_dir = self.data_dir / "input"
        self.output_dir = self.data_dir / "output"

        # Create directories if they don't exist
        self.input_dir.mkdir(parents=True, exist_ok=True)
        self.output_dir.mkdir(parents=True, exist_ok=True)

    def read_csv_safely(self, filename: str) -> Optional[pd.DataFrame]:
        """
        Read CSV with error handling.

        Learning points:
        - Always check if file exists
        - Handle encoding issues
        - Provide helpful error messages
        """
        file_path = self.input_dir / filename

        # Check if file exists
        if not file_path.exists():
            print(f"❌ File not found: {filename}")
            print(f"   Looking in: {self.input_dir}")
            print(f"   Available files: {list(self.input_dir.glob('*.csv'))}")
            return None

        try:
            # Try reading with common encodings
            for encoding in ['utf-8', 'latin-1', 'cp1252']:
                try:
                    df = pd.read_csv(file_path, encoding=encoding)
                    print(f"✅ Successfully read {filename} with {encoding} encoding")
                    return df
                except UnicodeDecodeError:
                    continue

            # If all encodings fail
            print(f"❌ Couldn't read {filename} - encoding issues")
            return None

        except Exception as e:
            print(f"❌ Error reading {filename}: {e}")
            return None

    def basic_data_info(self, df: pd.DataFrame) -> dict:
        """
        Get basic information about the dataframe.

        Learning points:
        - DataFrame properties
        - Data types
        - Missing values
        """
        info = {
            'rows': len(df),
            'columns': len(df.columns),
            'column_names': list(df.columns),
            'data_types': df.dtypes.to_dict(),
            'missing_values': df.isnull().sum().to_dict(),
            'memory_usage': f"{df.memory_usage(deep=True).sum() / 1024**2:.2f} MB"
        }

        return info

    def clean_basic(self, df: pd.DataFrame) -> pd.DataFrame:
        """
        Basic data cleaning operations.

        Learning points:
        - Remove duplicates
        - Handle missing values
        - Strip whitespace
        """
        # Make a copy to avoid changing original
        df_clean = df.copy()

        # Remove duplicate rows
        before_rows = len(df_clean)
        df_clean = df_clean.drop_duplicates()
        after_rows = len(df_clean)
        print(f"Removed {before_rows - after_rows} duplicate rows")

        # Strip whitespace from string columns
        for col in df_clean.select_dtypes(include=['object']).columns:
            df_clean[col] = df_clean[col].str.strip()

        # Basic missing value handling
        # (You might want different strategies for different columns)
        for col in df_clean.columns:
            missing_count = df_clean[col].isnull().sum()
            if missing_count > 0:
                print(f"Column '{col}' has {missing_count} missing values")

        return df_clean

    def save_processed(self, df: pd.DataFrame, filename: str):
        """Save processed data."""
        output_path = self.output_dir / filename

        try:
            df.to_csv(output_path, index=False, encoding='utf-8')
            print(f"✅ Saved to {output_path}")
            print(f"   Shape: {df.shape}")

        except Exception as e:
            print(f"❌ Couldn't save file: {e}")


# Example usage
if __name__ == "__main__":
    # Create processor
    processor = SimpleDataProcessor()

    # Read data
    df = processor.read_csv_safely("sample_data.csv")

    if df is not None:
        # Show info
        info = processor.basic_data_info(df)
        print(f"\nData Info:")
        print(f"  Rows: {info['rows']}")
        print(f"  Columns: {info['columns']}")

        # Clean data
        df_clean = processor.clean_basic(df)

        # Save results
        processor.save_processed(df_clean, "cleaned_data.csv")
```

### Small Web App Pattern

```python
# File: /app/app_i_am_developing/src/web/simple_app.py

from flask import Flask, render_template, request, jsonify
from pathlib import Path
import json


# Create Flask app
app = Flask(__name__)


# Configuration
class Config:
    """Simple configuration for the app."""
    PROJECT_ROOT = Path("/app/app_i_am_developing")
    DATA_DIR = PROJECT_ROOT / "data"
    UPLOAD_FOLDER = DATA_DIR / "uploads"
    MAX_FILE_SIZE = 5 * 1024 * 1024  # 5MB


# Ensure directories exist
Config.UPLOAD_FOLDER.mkdir(parents=True, exist_ok=True)


@app.route('/')
def home():
    """
    Home page.

    Learning points:
    - Route decorators
    - Template rendering
    - Function returns
    """
    return render_template('index.html', title='My Simple App')


@app.route('/api/process', methods=['POST'])
def process_data():
    """
    Process data endpoint.

    Learning points:
    - POST request handling
    - JSON responses
    - Error handling in web apps
    """
    try:
        # Get data from request
        data = request.get_json()

        if not data:
            return jsonify({
                'status': 'error',
                'message': 'No data provided'
            }), 400

        # Simple processing example
        text = data.get('text', '')

        # Do something with the text
        result = {
            'original': text,
            'length': len(text),
            'words': len(text.split()),
            'uppercase': text.upper()
        }

        return jsonify({
            'status': 'success',
            'result': result
        })

    except Exception as e:
        # Log error for debugging
        app.logger.error(f"Error processing data: {e}")

        return jsonify({
            'status': 'error',
            'message': 'Something went wrong'
        }), 500


@app.route('/api/stats')
def get_stats():
    """
    Get simple statistics.

    Learning points:
    - GET endpoints
    - File operations in web context
    - JSON responses
    """
    try:
        # Count files in upload directory
        upload_count = len(list(Config.UPLOAD_FOLDER.glob('*')))

        stats = {
            'upload_count': upload_count,
            'max_file_size_mb': Config.MAX_FILE_SIZE / (1024 * 1024),
            'data_directory': str(Config.DATA_DIR)
        }

        return jsonify(stats)

    except Exception as e:
        app.logger.error(f"Error getting stats: {e}")
        return jsonify({'error': 'Could not get stats'}), 500


# Error handlers
@app.errorhandler(404)
def not_found(error):
    """Handle 404 errors nicely."""
    return jsonify({
        'status': 'error',
        'message': 'Page not found'
    }), 404


if __name__ == '__main__':
    # Run the app in debug mode for development
    # Note: In production, use a proper WSGI server
    app.run(host='0.0.0.0', port=5000, debug=True)
```

## Error Handling Patterns

### Progressive Error Handling

```python
# Level 1: Basic try/except
def basic_error_handling():
    try:
        result = risky_operation()
        return result
    except Exception as e:
        print(f"Error: {e}")
        return None

# Level 2: Specific exceptions
def better_error_handling():
    try:
        result = risky_operation()
        return result
    except FileNotFoundError:
        print("File not found - check the path")
        return None
    except ValueError as e:
        print(f"Invalid value: {e}")
        return None
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None

# Level 3: Error context and recovery
def advanced_error_handling(filepath):
    """Error handling with context and recovery options."""
    try:
        with open(filepath, 'r') as f:
            data = json.load(f)
        return data

    except FileNotFoundError:
        print(f"❌ File not found: {filepath}")
        print(f"   Creating empty file...")

        # Try to create the file
        Path(filepath).parent.mkdir(parents=True, exist_ok=True)
        with open(filepath, 'w') as f:
            json.dump({}, f)

        print(f"✅ Created {filepath}")
        return {}

    except json.JSONDecodeError as e:
        print(f"❌ Invalid JSON in {filepath}")
        print(f"   Error at line {e.lineno}, column {e.colno}")
        print(f"   Creating backup and starting fresh...")

        # Backup corrupted file
        backup_path = f"{filepath}.backup"
        Path(filepath).rename(backup_path)

        return {}
```

## Configuration Management

### Simple Configuration Pattern

```python
# File: /app/app_i_am_developing/config/settings.py

import os
from pathlib import Path
from dotenv import load_dotenv

# Load environment variables
load_dotenv()


class Settings:
    """
    Application settings.

    Learning points:
    - Environment variables for secrets
    - Default values for safety
    - Type conversion for env vars
    """

    # Project paths
    PROJECT_ROOT = Path("/app/app_i_am_developing")
    DATA_DIR = PROJECT_ROOT / "data"
    LOG_DIR = PROJECT_ROOT / "logs"

    # API Configuration
    GEMINI_API_KEY = os.getenv('GEMINI_API_KEY', '')
    OPENAI_API_KEY = os.getenv('OPENAI_API_KEY', '')

    # App Configuration
    DEBUG = os.getenv('DEBUG', 'True').lower() == 'true'
    LOG_LEVEL = os.getenv('LOG_LEVEL', 'INFO')

    # Data Processing
    MAX_FILE_SIZE_MB = int(os.getenv('MAX_FILE_SIZE_MB', '10'))
    BATCH_SIZE = int(os.getenv('BATCH_SIZE', '100'))

    # Web Scraping
    SCRAPE_DELAY = float(os.getenv('SCRAPE_DELAY', '1.0'))
    USER_AGENT = os.getenv('USER_AGENT', 'Personal Learning Bot')

    @classmethod
    def validate(cls):
        """Check if required settings are present."""
        errors = []

        # Check required API keys
        if not cls.GEMINI_API_KEY and not cls.OPENAI_API_KEY:
            errors.append("No LLM API key found")

        # Check directories exist
        for dir_path in [cls.DATA_DIR, cls.LOG_DIR]:
            if not dir_path.exists():
                dir_path.mkdir(parents=True, exist_ok=True)
                print(f"Created directory: {dir_path}")

        if errors:
            print("⚠️  Configuration issues:")
            for error in errors:
                print(f"   - {error}")

        return len(errors) == 0


# Create global settings instance
settings = Settings()

# Validate on import
if __name__ == "__main__":
    if settings.validate():
        print("✅ Configuration valid")
    else:
        print("❌ Configuration has issues")
```

## Documentation Standards

### Function Documentation Template

```python
def process_user_data(user_id: str, options: dict = None) -> dict:
    """
    Process user data with given options.

    This function demonstrates:
    - Type hints for clarity
    - Default parameter values
    - Dictionary operations
    - Error handling

    Args:
        user_id (str): The user's unique identifier
        options (dict, optional): Processing options. Defaults to None.
            Supported options:
            - 'validate': bool - Whether to validate data
            - 'format': str - Output format ('json' or 'dict')

    Returns:
        dict: Processed user data with keys:
            - 'user_id': str - The user ID
            - 'processed': bool - Whether processing succeeded
            - 'data': dict - The processed data

    Example:
        >>> result = process_user_data("user123", {'validate': True})
        >>> print(result['processed'])
        True

    Note:
        This is a learning example - in real apps, you'd want
        more robust error handling and validation.
    """
    # Set default options
    if options is None:
        options = {}

    # Implementation here
    pass
```

### Module Documentation Template

```python
"""
data_analyzer.py - Simple data analysis utilities

This module teaches:
- Basic pandas operations
- Data visualization
- Statistical summaries
- File I/O patterns

Requirements:
    - pandas
    - matplotlib
    - numpy

Usage:
    from data_analyzer import analyze_csv

    results = analyze_csv("sales_data.csv")
    print(results['summary'])

Classes:
    DataAnalyzer: Main analysis class

Functions:
    analyze_csv(): Quick CSV analysis
    plot_trends(): Create trend plots

Constants:
    SUPPORTED_FORMATS: List of supported file types
    DEFAULT_PLOT_SIZE: Default plot dimensions

Author: Your Name
Created: Date
Modified: Date
"""
```

## Code Review Checklist

Before considering code complete, check:

### Readability

- [ ] Variable names clearly describe their purpose
- [ ] Functions have single, clear responsibilities
- [ ] Complex logic has explanatory comments
- [ ] No clever one-liners that are hard to understand

### Documentation

- [ ] All functions have docstrings with examples
- [ ] Module has overview documentation
- [ ] Inline comments explain "why" not "what"
- [ ] README updated with new features

### Error Handling

- [ ] All file operations have try/except blocks
- [ ] API calls handle network errors
- [ ] User input is validated
- [ ] Error messages are helpful

### Learning Value

- [ ] Code demonstrates Python concepts clearly
- [ ] New concepts have explanatory comments
- [ ] Examples show real use cases
- [ ] Progressive complexity when appropriate

### Container Compatibility

- [ ] All paths use Path() objects
- [ ] Paths are absolute from /app/app_i_am_developing
- [ ] No hardcoded local machine paths
- [ ] Works with Docker volume mounts

Remember: The goal is to write code that teaches you Python while solving real problems. Every line should be clear, every function should be understandable, and every module should make you a better programmer.
