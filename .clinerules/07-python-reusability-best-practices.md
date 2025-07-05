# Best Practices for Developing Reusable Python Code

## Executive Summary

This comprehensive guide covers essential best practices for developing reusable Python code that can be easily maintained, shared, and extended. Based on current industry standards and modern Python 3.8+ practices, this document provides actionable guidance across eleven critical areas of Python development, from code organization and testing to security, performance optimization, and building robust API wrappers for multi-provider architectures.

## 1. Code Structure and Organization

### Project Layout and Structure

**Recommended: src/ Layout**
The src/ layout has become the preferred structure for new Python projects due to its advantages in testing and packaging:

```
my-project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       ├── core/
│       │   ├── __init__.py
│       │   ├── models.py
│       │   └── services.py
│       ├── api/
│       │   ├── __init__.py
│       │   └── endpoints.py
│       └── utils/
│           ├── __init__.py
│           └── helpers.py
├── tests/
│   ├── __init__.py
│   ├── unit/
│   └── integration/
├── docs/
├── pyproject.toml
├── README.md
└── LICENSE
```

**Key Benefits:**

- Forces proper package installation for testing
- Prevents accidental imports from development directory
- Clearer separation between source code and project files
- Better for team collaboration and CI/CD workflows

### Module Organization Principles

**Hierarchical Structure by Functionality**

- **Single Responsibility**: Each module should have one clear purpose
- **Logical Grouping**: Group related functionality together
- **Avoid Circular Imports**: Structure dependencies hierarchically
- **Strategic **init**.py Usage**: Control package namespace and expose key APIs

**Example Module Structure:**

```python
# Package initialization with controlled exports
# src/myapp/__init__.py
from .core.models import User, Order
from .core.services import UserService, OrderService
from .exceptions import MyAppError, ValidationError

__version__ = "1.0.0"
__all__ = ["User", "Order", "UserService", "OrderService", "MyAppError"]
```

### Modern Project Configuration

**pyproject.toml - The Modern Standard**

```toml
[build-system]
requires = ["setuptools>=64", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-package"
version = "1.0.0"
description = "A reusable Python package"
readme = "README.md"
requires-python = ">=3.8"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "your.email@example.com"},
]
classifiers = [
    "Development Status :: 4 - Beta",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
]
dependencies = [
    "requests>=2.25.0",
    "pydantic>=1.8.0",
]

[project.optional-dependencies]
dev = [
    "ruff>=0.4.0",
    "mypy>=1.0.0",
    "pytest>=7.0.0",
    "black>=24.0.0",
]
```

## 2. Code Style and Formatting Standards

### Modern Tooling with Ruff

**Ruff** has emerged as the fastest Python linter and formatter, offering 10-100x performance improvements over traditional tools:

```toml
[tool.ruff]
line-length = 88
target-version = "py38"

[tool.ruff.lint]
select = [
    "E",    # pycodestyle errors
    "W",    # pycodestyle warnings
    "F",    # Pyflakes
    "I",    # isort
    "B",    # flake8-bugbear
    "C4",   # flake8-comprehensions
    "UP",   # pyupgrade
]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
```

### Code Quality Standards

**Essential Quality Checks:**

- **Linting**: Use Ruff for fast, comprehensive code analysis
- **Type Checking**: Implement mypy for static type checking
- **Pre-commit Hooks**: Automate quality checks with pre-commit
- **CI/CD Integration**: Enforce standards in automated pipelines

**Pre-commit Configuration:**

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.4.7
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.5.1
    hooks:
      - id: mypy
```

## 3. Documentation Standards and API Design

### Documentation Best Practices

**Google-Style Docstrings** (Recommended)

```python
def calculate_metrics(data: List[Dict[str, Any]],
                     threshold: float = 0.5) -> Dict[str, float]:
    """Calculate performance metrics from data.

    Args:
        data: List of dictionaries containing measurement data.
        threshold: Minimum value threshold for filtering.

    Returns:
        Dictionary containing calculated metrics.

    Raises:
        ValueError: If data is empty or contains invalid values.

    Example:
        >>> data = [{"value": 0.8, "weight": 1.0}]
        >>> calculate_metrics(data)
        {"weighted_average": 0.8, "count": 1}
    """
```

**Type Hints as Documentation**
Modern Python leverages type hints for improved documentation and IDE support:

```python
from typing import List, Dict, Optional, Union
from pathlib import Path

def process_data(
    data: List[Dict[str, Union[str, int]]],
    output_file: Optional[Path] = None,
    validate: bool = True
) -> Dict[str, int]:
    """Process and validate input data."""
    # Implementation
    pass
```

### API Design Principles

**Core Design Principles:**

1. **Simplicity**: Follow the "requests" library model - minimal boilerplate
2. **Consistency**: Use consistent naming and parameter patterns
3. **Pythonic Design**: Leverage Python idioms and conventions
4. **Context Managers**: Use for resource management

**Example of Good API Design:**

```python
class DataProcessor:
    def __init__(self, source: str, options: Optional[Dict] = None):
        self.source = source
        self.options = options or {}

    def process(self) -> List[Dict]:
        """Process data and return results."""
        # Implementation
        pass

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.cleanup()

# Usage
with DataProcessor("data.csv") as processor:
    results = processor.process()
```

**API Design Anti-Patterns to Avoid:**

- Inconsistent return types
- Mutable default arguments
- "God objects" that do too much
- Breaking changes without deprecation warnings

## 4. Testing Strategies for Reusable Code

### Testing Framework Selection

**pytest** is the recommended testing framework for modern Python projects:

**Key Advantages:**

- Simplified syntax with plain `assert` statements
- Rich plugin ecosystem (800+ plugins)
- Powerful fixtures for dependency injection
- Excellent error reporting with detailed introspection

**Basic pytest Configuration:**

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py", "*_test.py"]
addopts = [
    "--strict-markers",
    "--strict-config",
    "--cov=src",
    "--cov-report=html",
    "--cov-report=term-missing",
]
```

### Testing Strategies

**Test Organization Structure:**

```
tests/
├── __init__.py
├── conftest.py              # pytest configuration
├── unit/
│   ├── test_models.py
│   └── test_services.py
├── integration/
│   └── test_api.py
└── fixtures/
    └── test_data.py
```

**Testing Best Practices:**

1. **Test Behavior, Not Implementation**: Focus on what the code does
2. **Test Multiple Use Cases**: Different inputs, edge cases, error conditions
3. **Use Property-Based Testing**: Leverage Hypothesis for comprehensive testing
4. **Mock External Dependencies**: Isolate units under test
5. **Maintain High Coverage**: Target 80%+ with meaningful tests

**Example Test Implementation:**

```python
import pytest
from unittest.mock import Mock, patch
from myapp.services import UserService

class TestUserService:
    @pytest.fixture
    def mock_database(self):
        return Mock()

    @pytest.fixture
    def user_service(self, mock_database):
        return UserService(database=mock_database)

    def test_get_user_success(self, user_service, mock_database):
        # Arrange
        mock_database.fetch_user.return_value = {"id": 1, "name": "John"}

        # Act
        result = user_service.get_user(1)

        # Assert
        assert result["name"] == "John"
        mock_database.fetch_user.assert_called_once_with(1)
```

### Multi-Version Testing

**tox Configuration for Multi-Version Testing:**

```ini
[tox]
envlist = py38,py39,py310,py311,py312
isolated_build = True

[testenv]
deps =
    pytest
    pytest-cov
    -r requirements.txt
commands =
    pytest {posargs}
```

## 5. Packaging and Distribution

### Modern Packaging Standards

**Current Python Packaging is Built on:**

- **PEP 517**: Build backend interface
- **PEP 518**: Build system dependencies
- **PEP 621**: Project metadata in pyproject.toml

**Build Backend Comparison:**

- **Setuptools**: Most compatible, largest ecosystem
- **Hatchling**: Modern, fast, comprehensive tooling
- **Poetry**: Full dependency management with locking
- **Flit**: Minimal overhead for pure Python packages

### Distribution Workflow

**Automated Distribution with GitHub Actions:**

```yaml
name: Publish to PyPI

on:
  release:
    types: [published]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'
    - name: Build package
      run: |
        pip install build
        python -m build
    - name: Publish to PyPI
      uses: pypa/gh-action-pypi-publish@release/v1
      with:
        user: __token__
        password: ${{ secrets.PYPI_API_TOKEN }}
```

## 6. Version Management and Backwards Compatibility

### Semantic Versioning

**Version Format**: `MAJOR.MINOR.PATCH`

- **MAJOR**: Incompatible API changes
- **MINOR**: Backwards-compatible functionality additions
- **PATCH**: Backwards-compatible bug fixes

**Python-Specific Versioning (PEP 440):**

```python
# Version examples
"1.0.0"        # Release
"1.0.0a1"      # Alpha pre-release
"1.0.0b2"      # Beta pre-release
"1.0.0rc1"     # Release candidate
"1.0.0.post1"  # Post-release
```

### Deprecation Strategy

**Proper Deprecation Implementation:**

```python
import warnings
from functools import wraps

def deprecated(reason="This function is deprecated"):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            warnings.warn(
                f"{func.__name__} is deprecated. {reason}",
                DeprecationWarning,
                stacklevel=2
            )
            return func(*args, **kwargs)
        return wrapper
    return decorator

@deprecated("Use new_function() instead")
def old_function():
    pass
```

**Deprecation Timeline:**

1. **Warning Phase**: Minimum 2 years (2 Python versions)
2. **Documentation**: Update docs and changelog
3. **Migration Path**: Provide clear upgrade instructions
4. **Removal**: Only after sufficient warning period

## 7. Error Handling and Logging

### Exception Handling Best Practices

**Custom Exception Hierarchy:**

```python
class MyLibraryError(Exception):
    """Base exception for MyLibrary."""
    pass

class ValidationError(MyLibraryError):
    """Raised when input validation fails."""
    pass

class ConfigurationError(MyLibraryError):
    """Raised when configuration is invalid."""
    pass
```

**Proper Exception Handling:**

```python
try:
    result = process_data(user_input)
except ValueError as e:
    logger.error(f"Invalid input format: {e}")
    raise ValidationError("Input must be valid format") from e
except FileNotFoundError as e:
    logger.error(f"Required file not found: {e}")
    raise ConfigurationError("Configuration file missing") from e
```

### Logging Best Practices

**Structured Logging Configuration:**

```python
import logging
import json
from datetime import datetime

class JSONFormatter(logging.Formatter):
    def format(self, record):
        log_entry = {
            'timestamp': datetime.utcnow().isoformat(),
            'level': record.levelname,
            'logger': record.name,
            'message': record.getMessage(),
            'module': record.module,
            'function': record.funcName,
            'line': record.lineno
        }
        if record.exc_info:
            log_entry['exception'] = self.formatException(record.exc_info)
        return json.dumps(log_entry)

# Module-level logger
logger = logging.getLogger(__name__)
```

**Security-Conscious Logging:**

```python
def sanitize_log_data(data):
    """Remove sensitive information from log data."""
    import re
    # Remove credit card numbers
    data = re.sub(r'\b\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}\b',
                  '[CARD-REDACTED]', data)
    # Remove passwords
    data = re.sub(r'password["\']?\s*[:=]\s*["\']?[^\s"\']+',
                  'password=[REDACTED]', data, flags=re.IGNORECASE)
    return data
```

## 8. Security Best Practices

### Input Validation and Sanitization

**Comprehensive Input Validation:**

```python
import re
from typing import Union
from urllib.parse import urlparse

def validate_email(email: str) -> bool:
    """Validate email format using regex."""
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

def validate_url(url: str) -> bool:
    """Validate URL format and scheme."""
    try:
        parsed = urlparse(url)
        return parsed.scheme in ('http', 'https') and parsed.netloc
    except Exception:
        return False

def sanitize_filename(filename: str) -> str:
    """Sanitize filename to prevent directory traversal."""
    filename = re.sub(r'[/\\:*?"<>|]', '_', filename)
    filename = filename.strip('. ')
    return filename[:255]
```

### Secure Coding Practices

**Cryptographic Operations:**

```python
import secrets
import hashlib
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
import base64

def generate_secure_token(length: int = 32) -> str:
    """Generate cryptographically secure random token."""
    return secrets.token_urlsafe(length)

def hash_password(password: str, salt: bytes = None) -> tuple[str, bytes]:
    """Hash password using PBKDF2."""
    if salt is None:
        salt = secrets.token_bytes(32)

    kdf = PBKDF2HMAC(
        algorithm=hashes.SHA256(),
        length=32,
        salt=salt,
        iterations=100000,
    )
    key = base64.urlsafe_b64encode(kdf.derive(password.encode()))
    return key.decode(), salt
```

### Security Analysis Tools

**Essential Security Tools:**

- **Bandit**: Static security analyzer for Python
- **Safety**: Dependency vulnerability scanner
- **pip-audit**: Official PyPA vulnerability scanner
- **Semgrep**: Advanced static analysis with security rules

**CI/CD Security Integration:**

```yaml
# Security scanning in GitHub Actions
- name: Run Bandit
  run: bandit -r . -f json -o bandit-report.json

- name: Run Safety
  run: safety check --json
```

## 9. Performance Considerations

### Core Optimization Strategies

**Algorithm and Data Structure Selection:**

- **Dictionaries/Sets**: O(1) lookup time for membership testing
- **Lists**: O(1) append/pop from end, O(n) from beginning
- **Tuples**: Faster construction and lower memory than lists
- **Generators**: Memory-efficient for large datasets

**Built-in Function Utilization:**

```python
# Optimized approaches
result = sum(x for x in data if x > threshold)  # Generator expression
sorted_data = sorted(data, key=operator.attrgetter('value'))  # Built-in sort

# Memory-efficient data processing
def process_large_dataset(filename):
    with open(filename) as f:
        for line in f:
            yield process_line(line)
```

### Profiling and Performance Monitoring

**Essential Profiling Tools:**

- **cProfile**: Built-in deterministic profiling
- **line_profiler**: Line-by-line performance analysis
- **memory_profiler**: Memory usage tracking
- **Scalene**: Combined CPU and memory profiling

**Performance Testing:**

```python
import timeit
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_function(n):
    # Expensive computation with caching
    return result

# Benchmark different approaches
def benchmark_functions():
    list_time = timeit.timeit(lambda: [x**2 for x in range(1000)], number=1000)
    gen_time = timeit.timeit(lambda: list(x**2 for x in range(1000)), number=1000)
    return list_time, gen_time
```

### Memory Optimization

**Memory-Efficient Patterns:**

```python
# Use __slots__ for memory optimization
class OptimizedClass:
    __slots__ = ['attribute1', 'attribute2', 'attribute3']

    def __init__(self, attr1, attr2, attr3):
        self.attribute1 = attr1
        self.attribute2 = attr2
        self.attribute3 = attr3

# Generator for lazy evaluation
def lazy_data_processor(data_source):
    for item in data_source:
        if meets_criteria(item):
            yield transform_item(item)
```

### Concurrency and Parallelism

**Concurrency Guidelines:**

- **I/O-bound tasks**: Use `asyncio` for best performance
- **CPU-bound tasks**: Use `multiprocessing` for true parallelism
- **Mixed workloads**: Combine approaches appropriately

**Async Implementation:**

```python
import asyncio
import aiohttp

async def fetch_data(session, url):
    async with session.get(url) as response:
        return await response.text()

async def process_multiple_urls(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_data(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
    return results
```

## 10. Building Robust API Wrappers

### Core Design Principles for API Wrappers

Creating effective API wrappers is crucial in today's multi-provider landscape, especially for services like LLMs where you might want to switch between OpenAI, Anthropic, Google, or local models.

### Abstract Base Classes for Consistency

Define clear interfaces that all providers must implement:

```python
from abc import ABC, abstractmethod
from typing import Dict, List, Optional, Union, AsyncIterator
from dataclasses import dataclass
from enum import Enum

class ModelCapability(Enum):
    TEXT_GENERATION = "text_generation"
    CHAT = "chat"
    FUNCTION_CALLING = "function_calling"
    VISION = "vision"
    STREAMING = "streaming"

@dataclass
class Message:
    role: str
    content: str
    metadata: Optional[Dict] = None

@dataclass
class ModelResponse:
    content: str
    model: str
    usage: Dict[str, int]
    metadata: Dict = None
    raw_response: Optional[Dict] = None

class LLMProvider(ABC):
    """Abstract base class for all LLM providers."""

    @property
    @abstractmethod
    def supported_capabilities(self) -> List[ModelCapability]:
        """Return list of capabilities this provider supports."""
        pass

    @property
    @abstractmethod
    def available_models(self) -> List[str]:
        """Return list of available models."""
        pass

    @abstractmethod
    async def generate(
        self,
        messages: List[Message],
        model: str,
        max_tokens: Optional[int] = None,
        temperature: float = 0.7,
        **kwargs
    ) -> ModelResponse:
        """Generate a response from the model."""
        pass

    @abstractmethod
    async def stream_generate(
        self,
        messages: List[Message],
        model: str,
        **kwargs
    ) -> AsyncIterator[str]:
        """Stream response tokens."""
        pass

    @abstractmethod
    async def health_check(self) -> bool:
        """Check if the provider is available."""
        pass
```

### Provider-Specific Implementations

Create concrete implementations for each provider:

```python
import openai
import anthropic
import aiohttp
from typing import AsyncIterator

class OpenAIProvider(LLMProvider):
    def __init__(self, api_key: str, base_url: Optional[str] = None):
        self.client = openai.AsyncOpenAI(
            api_key=api_key,
            base_url=base_url
        )
        self._models = [
            "gpt-4", "gpt-4-turbo", "gpt-3.5-turbo",
            "gpt-4-vision-preview"
        ]

    @property
    def supported_capabilities(self) -> List[ModelCapability]:
        return [
            ModelCapability.TEXT_GENERATION,
            ModelCapability.CHAT,
            ModelCapability.FUNCTION_CALLING,
            ModelCapability.VISION,
            ModelCapability.STREAMING
        ]

    @property
    def available_models(self) -> List[str]:
        return self._models

    async def generate(
        self,
        messages: List[Message],
        model: str,
        max_tokens: Optional[int] = None,
        temperature: float = 0.7,
        **kwargs
    ) -> ModelResponse:
        try:
            # Convert our Message format to OpenAI format
            openai_messages = [
                {"role": msg.role, "content": msg.content}
                for msg in messages
            ]

            response = await self.client.chat.completions.create(
                model=model,
                messages=openai_messages,
                max_tokens=max_tokens,
                temperature=temperature,
                **kwargs
            )

            return ModelResponse(
                content=response.choices[0].message.content,
                model=response.model,
                usage={
                    "prompt_tokens": response.usage.prompt_tokens,
                    "completion_tokens": response.usage.completion_tokens,
                    "total_tokens": response.usage.total_tokens
                },
                raw_response=response.model_dump()
            )
        except Exception as e:
            raise ProviderError(f"OpenAI API error: {str(e)}") from e

    async def stream_generate(
        self,
        messages: List[Message],
        model: str,
        **kwargs
    ) -> AsyncIterator[str]:
        openai_messages = [
            {"role": msg.role, "content": msg.content}
            for msg in messages
        ]

        stream = await self.client.chat.completions.create(
            model=model,
            messages=openai_messages,
            stream=True,
            **kwargs
        )

        async for chunk in stream:
            if chunk.choices[0].delta.content:
                yield chunk.choices[0].delta.content

    async def health_check(self) -> bool:
        try:
            await self.client.models.list()
            return True
        except Exception:
            return False

# Custom exceptions
class ProviderError(Exception):
    """Base exception for provider errors."""
    pass

class RateLimitError(ProviderError):
    """Raised when rate limit is exceeded."""
    pass

class ModelNotAvailableError(ProviderError):
    """Raised when requested model is not available."""
    pass
```

### Unified Client with Provider Management

Create a high-level client that manages multiple providers:

```python
import asyncio
from typing import Dict, Optional, List
import logging
from dataclasses import dataclass, field
from enum import Enum

class FallbackStrategy(Enum):
    FAIL_FAST = "fail_fast"
    TRY_ALL = "try_all"
    ROUND_ROBIN = "round_robin"
    COST_OPTIMIZED = "cost_optimized"

@dataclass
class ProviderConfig:
    name: str
    provider: LLMProvider
    priority: int = 1
    cost_per_token: float = 0.0
    rate_limit: Optional[int] = None
    enabled: bool = True
    model_mapping: Dict[str, str] = field(default_factory=dict)

class UnifiedLLMClient:
    """Unified client that manages multiple LLM providers."""

    def __init__(
        self,
        providers: Dict[str, ProviderConfig],
        fallback_strategy: FallbackStrategy = FallbackStrategy.TRY_ALL,
        default_provider: Optional[str] = None
    ):
        self.providers = providers
        self.fallback_strategy = fallback_strategy
        self.default_provider = default_provider
        self.logger = logging.getLogger(__name__)
        self._usage_stats = {}

    async def generate(
        self,
        messages: List[Message],
        model: str,
        provider: Optional[str] = None,
        max_tokens: Optional[int] = None,
        temperature: float = 0.7,
        **kwargs
    ) -> ModelResponse:
        """Generate response with automatic provider selection and fallback."""

        if provider:
            # Use specific provider
            provider_config = self.providers.get(provider)
            if not provider_config or not provider_config.enabled:
                raise ProviderError(f"Provider {provider} not available")

            return await self._try_provider(
                provider_config, messages, model, max_tokens, temperature, **kwargs
            )

        # Auto-select provider based on strategy
        providers_to_try = self._select_providers(model)

        last_error = None
        for provider_config in providers_to_try:
            try:
                return await self._try_provider(
                    provider_config, messages, model, max_tokens, temperature, **kwargs
                )
            except Exception as e:
                last_error = e
                self.logger.warning(
                    f"Provider {provider_config.name} failed: {str(e)}"
                )

                if self.fallback_strategy == FallbackStrategy.FAIL_FAST:
                    raise

                continue

        raise ProviderError(f"All providers failed. Last error: {last_error}")

    def _select_providers(self, model: str) -> List[ProviderConfig]:
        """Select providers based on strategy and model availability."""
        available_providers = [
            config for config in self.providers.values()
            if config.enabled and self._model_available(config, model)
        ]

        if self.fallback_strategy == FallbackStrategy.COST_OPTIMIZED:
            available_providers.sort(key=lambda x: x.cost_per_token)
        else:
            available_providers.sort(key=lambda x: x.priority)

        return available_providers

    def _model_available(self, provider_config: ProviderConfig, model: str) -> bool:
        """Check if model is available on provider."""
        available_models = provider_config.provider.available_models
        mapped_model = provider_config.model_mapping.get(model, model)
        return mapped_model in available_models

    async def _try_provider(
        self,
        provider_config: ProviderConfig,
        messages: List[Message],
        model: str,
        max_tokens: Optional[int] = None,
        temperature: float = 0.7,
        **kwargs
    ) -> ModelResponse:
        """Try a specific provider with error handling."""

        # Map model name if needed
        mapped_model = provider_config.model_mapping.get(model, model)

        start_time = asyncio.get_event_loop().time()
        try:
            response = await provider_config.provider.generate(
                messages, mapped_model, max_tokens, temperature, **kwargs
            )

            # Track usage statistics
            self._track_usage(provider_config.name, response, start_time)

            return response

        except Exception as e:
            self._track_error(provider_config.name, str(e))
            raise

    def _track_usage(self, provider_name: str, response: ModelResponse, start_time: float):
        """Track usage statistics."""
        if provider_name not in self._usage_stats:
            self._usage_stats[provider_name] = {
                "requests": 0,
                "tokens": 0,
                "errors": 0,
                "avg_latency": 0
            }

        stats = self._usage_stats[provider_name]
        stats["requests"] += 1
        stats["tokens"] += response.usage.get("total_tokens", 0)

        latency = asyncio.get_event_loop().time() - start_time
        stats["avg_latency"] = (stats["avg_latency"] + latency) / 2

    def _track_error(self, provider_name: str, error: str):
        """Track error statistics."""
        if provider_name not in self._usage_stats:
            self._usage_stats[provider_name] = {
                "requests": 0,
                "tokens": 0,
                "errors": 0,
                "avg_latency": 0
            }

        self._usage_stats[provider_name]["errors"] += 1

    def get_usage_stats(self) -> Dict:
        """Get usage statistics for all providers."""
        return self._usage_stats.copy()

    async def health_check_all(self) -> Dict[str, bool]:
        """Check health of all providers."""
        results = {}
        for name, config in self.providers.items():
            if config.enabled:
                results[name] = await config.provider.health_check()
            else:
                results[name] = False
        return results
```

### Advanced Features for API Wrappers

#### Retry Logic with Exponential Backoff

```python
import asyncio
from functools import wraps
import random

def retry_with_backoff(
    max_retries: int = 3,
    base_delay: float = 1.0,
    max_delay: float = 60.0,
    exponential_base: float = 2.0,
    jitter: bool = True
):
    """Decorator for retry logic with exponential backoff."""

    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            last_exception = None

            for attempt in range(max_retries + 1):
                try:
                    return await func(*args, **kwargs)
                except RateLimitError as e:
                    last_exception = e
                    if attempt == max_retries:
                        break

                    delay = min(
                        base_delay * (exponential_base ** attempt),
                        max_delay
                    )

                    if jitter:
                        delay *= (0.5 + random.random() * 0.5)

                    await asyncio.sleep(delay)
                except Exception as e:
                    # Don't retry for non-retryable errors
                    if not _is_retryable_error(e):
                        raise

                    last_exception = e
                    if attempt == max_retries:
                        break

                    delay = min(
                        base_delay * (exponential_base ** attempt),
                        max_delay
                    )
                    await asyncio.sleep(delay)

            raise last_exception

        return wrapper
    return decorator

def _is_retryable_error(error: Exception) -> bool:
    """Determine if an error is retryable."""
    retryable_errors = (
        RateLimitError,
        ConnectionError,
        TimeoutError,
    )
    return isinstance(error, retryable_errors)
```

#### Circuit Breaker Pattern

```python
from enum import Enum
import time

class CircuitState(Enum):
    CLOSED = "closed"      # Normal operation
    OPEN = "open"          # Failing, don't try
    HALF_OPEN = "half_open"  # Testing if recovered

class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = CircuitState.CLOSED

    async def call(self, func, *args, **kwargs):
        if self.state == CircuitState.OPEN:
            if time.time() - self.last_failure_time >= self.timeout:
                self.state = CircuitState.HALF_OPEN
            else:
                raise Exception("Circuit breaker is OPEN")

        try:
            result = await func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise

    def _on_success(self):
        self.failure_count = 0
        self.state = CircuitState.CLOSED

    def _on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN
```

#### Response Caching Layer

```python
from functools import lru_cache
import hashlib
import json
from typing import Optional
import redis

class ResponseCache:
    """Cache for LLM responses."""

    def __init__(self, redis_url: Optional[str] = None, ttl: int = 3600):
        self.ttl = ttl
        if redis_url:
            self.redis_client = redis.from_url(redis_url)
        else:
            self.redis_client = None
        self._local_cache = {}

    def _generate_key(self, messages: List[Message], model: str, **kwargs) -> str:
        """Generate cache key from request parameters."""
        cache_data = {
            "messages": [{"role": msg.role, "content": msg.content} for msg in messages],
            "model": model,
            **kwargs
        }
        cache_string = json.dumps(cache_data, sort_keys=True)
        return hashlib.md5(cache_string.encode()).hexdigest()

    async def get(self, messages: List[Message], model: str, **kwargs) -> Optional[ModelResponse]:
        """Get cached response."""
        key = self._generate_key(messages, model, **kwargs)

        if self.redis_client:
            try:
                cached = await self.redis_client.get(key)
                if cached:
                    data = json.loads(cached)
                    return ModelResponse(**data)
            except Exception:
                pass

        # Fallback to local cache
        return self._local_cache.get(key)

    async def set(
        self,
        messages: List[Message],
        model: str,
        response: ModelResponse,
        **kwargs
    ):
        """Cache response."""
        key = self._generate_key(messages, model, **kwargs)

        if self.redis_client:
            try:
                await self.redis_client.setex(
                    key,
                    self.ttl,
                    json.dumps(response.__dict__)
                )
            except Exception:
                pass

        # Also cache locally
        self._local_cache[key] = response

        # Limit local cache size
        if len(self._local_cache) > 1000:
            # Remove oldest entries
            keys_to_remove = list(self._local_cache.keys())[:100]
            for k in keys_to_remove:
                del self._local_cache[k]
```

### Configuration Management for API Wrappers

```python
from pydantic import BaseSettings, SecretStr
from typing import Dict, Optional
import yaml
import os

class LLMConfig(BaseSettings):
    """Configuration for LLM providers."""

    # OpenAI Config
    openai_api_key: Optional[SecretStr] = None
    openai_base_url: Optional[str] = None
    openai_enabled: bool = True

    # Anthropic Config
    anthropic_api_key: Optional[SecretStr] = None
    anthropic_enabled: bool = True

    # Azure OpenAI Config
    azure_openai_api_key: Optional[SecretStr] = None
    azure_openai_endpoint: Optional[str] = None
    azure_openai_api_version: str = "2023-12-01-preview"
    azure_enabled: bool = False

    # Local Model Config
    local_model_url: Optional[str] = None
    local_enabled: bool = False

    # General Config
    default_provider: str = "openai"
    fallback_strategy: str = "try_all"
    rate_limit_requests_per_minute: int = 60
    timeout_seconds: int = 30

    class Config:
        env_file = ".env"
        case_sensitive = False

def create_unified_client(config: LLMConfig) -> UnifiedLLMClient:
    """Factory function to create UnifiedLLMClient from configuration."""

    providers = {}

    # Initialize OpenAI provider
    if config.openai_enabled and config.openai_api_key:
        providers["openai"] = ProviderConfig(
            name="openai",
            provider=OpenAIProvider(
                api_key=config.openai_api_key.get_secret_value(),
                base_url=config.openai_base_url
            ),
            priority=1,
            cost_per_token=0.00003,  # Example cost
            rate_limit=config.rate_limit_requests_per_minute,
            model_mapping={
                "gpt-4": "gpt-4",
                "gpt-3.5": "gpt-3.5-turbo"
            }
        )

    return UnifiedLLMClient(
        providers=providers,
        fallback_strategy=FallbackStrategy(config.fallback_strategy),
        default_provider=config.default_provider
    )
```

### Usage Examples for API Wrappers

```python
# Example configuration
config = LLMConfig(
    openai_api_key="sk-...",
    anthropic_api_key="sk-ant-...",
    default_provider="openai",
    fallback_strategy="cost_optimized"
)

# Create client
client = create_unified_client(config)

# Basic usage
messages = [
    Message(role="system", content="You are a helpful assistant."),
    Message(role="user", content="What is the capital of France?")
]

# Use default provider with fallback
response = await client.generate(
    messages=messages,
    model="gpt-4",
    max_tokens=100
)

# Use specific provider
response = await client.generate(
    messages=messages,
    model="claude-3",
    provider="anthropic",
    max_tokens=100
)

# Streaming
async for token in client.stream_generate(
    messages=messages,
    model="gpt-4"
):
    print(token, end="", flush=True)

# Health check
health_status = await client.health_check_all()
print(f"Provider health: {health_status}")

# Usage statistics
stats = client.get_usage_stats()
print(f"Usage stats: {stats}")
```

## 11. Implementation Roadmap

### Phase 1: Foundation (Immediate)

1. **Project Structure**: Implement src/ layout with pyproject.toml
2. **Code Quality**: Set up Ruff, mypy, and pre-commit hooks
3. **Testing**: Configure pytest with basic test structure
4. **Documentation**: Establish docstring standards and README

### Phase 2: Enhancement (Short-term)

1. **CI/CD Pipeline**: Implement automated testing and quality checks
2. **Security**: Add security scanning and input validation
3. **Performance**: Add profiling and performance testing
4. **Packaging**: Set up automated distribution workflow

### Phase 3: Advanced (Long-term)

1. **Performance Optimization**: Implement caching and advanced optimizations
2. **Security Hardening**: Add comprehensive security measures
3. **API Wrappers**: Implement robust multi-provider API wrapper patterns
4. **Monitoring**: Implement production monitoring and alerting
5. **Documentation**: Create comprehensive documentation site

### Essential Checklist for Reusable Code

**Code Organization:**

- [ ] Proper project structure with src/ layout
- [ ] Clear module organization and imports
- [ ] Consistent naming conventions
- [ ] Well-defined public APIs

**Quality Assurance:**

- [ ] Comprehensive test suite with good coverage
- [ ] Automated code formatting and linting
- [ ] Type hints throughout codebase
- [ ] Security scanning integration

**Documentation:**

- [ ] Complete docstrings for all public APIs
- [ ] Usage examples and tutorials
- [ ] Clear README with installation instructions
- [ ] API documentation generation

**Distribution:**

- [ ] Proper semantic versioning
- [ ] Automated packaging and distribution
- [ ] Dependency management
- [ ] Backwards compatibility strategy

**API Design (when applicable):**

- [ ] Abstract base classes for provider interfaces
- [ ] Unified client with fallback strategies
- [ ] Retry logic and circuit breakers
- [ ] Comprehensive error handling
- [ ] Response caching mechanisms

**Maintenance:**

- [ ] Performance monitoring and profiling
- [ ] Security vulnerability scanning
- [ ] Regular dependency updates
- [ ] Clear deprecation and migration paths

## Conclusion

Developing reusable Python code requires a systematic approach that balances code quality, performance, security, and maintainability. Whether you're building general-purpose libraries or complex multi-provider API wrappers, following these modern best practices and leveraging Python's rich ecosystem of tools enables you to create robust, scalable, and maintainable code that serves both current needs and future requirements.

**Key Success Factors:**

- Start with solid foundations (structure, testing, documentation)
- Implement automation early (CI/CD, quality checks, security scanning)
- Focus on user experience (clear APIs, good documentation, easy installation)
- Plan for flexibility (provider switching, fallback strategies, extensibility)
- Maintain code quality over time (regular updates, performance monitoring)
- Plan for evolution (deprecation strategies, backwards compatibility)

**API Wrapper Specific Benefits:**

- **Flexibility**: Easy to add new providers without changing application code
- **Reliability**: Built-in fallback and retry mechanisms
- **Observability**: Comprehensive metrics and logging for production use
- **Performance**: Caching and rate limiting optimizations
- **Cost Optimization**: Provider selection based on cost and capabilities
- **Testability**: Clear interfaces for mocking and testing

The investment in following these practices pays dividends in reduced maintenance costs, faster development cycles, increased adoption, and higher code quality across the entire development lifecycle. By combining general Python best practices with robust API wrapper patterns, you can build systems that are both reliable and adaptable to the rapidly changing landscape of external services and APIs.
