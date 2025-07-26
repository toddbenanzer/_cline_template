# Prompting Guidelines

## Core Principles for Prompting

- **Provide Context**: Always state skill level (4/10), environment (VS Code Remote Container), and goal
- **Be Specific**: Ask for exactly what you want (e.g., "function that reads CSV" not "process data")
- **Request Learning Explanations**: Explicitly ask for explanations of Python concepts, patterns, and errors
- **Reference Interaction Protocol**: All responses should follow standards from interaction protocol

## Standard Prompting Patterns

### Basic Code Generation
State you're a Python beginner (4/10) in VS Code Remote Container at `{PROJECT_ROOT}`. Describe task in simple terms. Request solution following clinerules with simple code, learning explanations, and clear testing instructions.

### Debugging Assistance
Explain goal, provide complete code snippet, paste full error message. Request explanation in simple terms, corrected code, and relevant Python concepts to study.

### Code Improvement
Share working code and request refactoring for better readability and error handling following project standards. Ask for explanation of each change and underlying Python principles.

### Project-Specific Prompting

**Web Scraping**: Request script for specific data from URL using `requests` and `BeautifulSoup`. Specify need for polite delays, error handling, and CSV output to `{PROJECT_ROOT}/data/output/`.

**Data Analysis**: Request script to analyze CSV in `{PROJECT_ROOT}/data/input/`. Specify pandas usage, basic cleaning steps, specific metrics to calculate, and summary output.

### Multi-Step Workflow Prompting

Build features incrementally with series of prompts:
1. "Create basic function that [does core task] - don't worry about error handling yet"
2. "Add try...except blocks to handle potential errors"
3. "Add logging so we can see what it's doing"

This aligns with Progressive Complexity pattern and makes learning manageable.

## Key Reminders

- Always request complete, runnable code (no "..." or omissions)
- Ask for explanations of new concepts and patterns
- Request testing instructions and verification steps
- Specify learning goals and skill level context
- Reference established project patterns and standards
