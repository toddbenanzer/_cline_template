# Technical Standards

## Environment & Paths

### Path Handling
- **Use `{PROJECT_ROOT}` placeholder**: Current value `c:/developer/_cline_template`
- **Always use `pathlib`**: `from pathlib import Path` for all file operations
- **Build paths from root**: `project_root = Path("{PROJECT_ROOT}")`, then `data_dir = project_root / "data"`

### Standard Project Structure
```
{PROJECT_ROOT}/
├── .devcontainer/   # VS Code Remote Container config
├── config/          # Configuration files
├── data/
│   ├── input/       # Raw input data
│   └── output/      # Processed results
├── docs/            # Documentation
├── examples/        # Usage examples
├── memory-bank/     # Cline's memory files
├── notebooks/       # Jupyter exploration
├── scripts/         # Automation utilities
├── src/             # Main source code
└── tests/           # Test files
```

### Environment Validation
Key checks before significant work:
- Working directory is `{PROJECT_ROOT}`
- Python version 3.12+
- `src` directory exists
- Write permissions to `data/output`

## Python Coding Standards

### Readability
- **Variables**: Descriptive snake_case (`user_email`, `retry_count`)
- **Functions**: Descriptive snake_case verbs (`fetch_user_data`, `validate_input`)
- **Classes**: PascalCase nouns (`WebScraper`, `DataProcessor`)
- **Constants**: ALL_CAPS snake_case (`MAX_RETRIES`, `DEFAULT_TIMEOUT`)
- **File Structure**: Module docstring, grouped imports (standard → third-party → local), function docstrings, single responsibility

### Essential Patterns

**Error Handling**:
- Use `try...except` blocks for operations that might fail
- Catch specific exceptions (`FileNotFoundError`, `ValueError`) when possible
- Provide helpful error messages with context

**Configuration Management**:
- Store secrets in `.env` files (must be in `.gitignore`)
- Use `config.py` with Settings class for centralized configuration

**Input Validation**:
- Always validate external data (user input, API responses, files)
- Check presence, type, and format before processing

### Advanced Practices (Optional)
- **Custom Exceptions**: For larger projects, define exception hierarchy
- **Type Hinting**: Use for all function signatures to improve clarity
- **Versioning**: Use Semantic Versioning for reusable tools

## Testing & Quality

### Testing Philosophy
- **Test for Learning**: Every test should answer a question about code behavior
- **Start Simple**: Begin with manual `print()` tests before automation
- **Practicality Over Coverage**: Focus on critical paths and likely failures

### Progressive Testing
1. **Manual Testing**: Use `print()` statements to observe behavior with different inputs
2. **Assert-Based**: Formalize with `assert` statements for self-verification
3. **Real Data Testing**: Test with representative samples in `tests/fixtures/`

### Quality Checks
- Simple project quality validation (README exists, requirements.txt, .env in .gitignore)
- Code structure validation

### Debugging Strategy
1. **Isolate**: Create smallest code that reproduces error
2. **Observe**: Add `print()` statements to check variable states and types
3. **Document**: Maintain ErrorJournal in docs folder for learning from mistakes
