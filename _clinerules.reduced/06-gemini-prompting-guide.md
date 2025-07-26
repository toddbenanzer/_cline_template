# Gemini Prompting Guide

This guide provides templates for interacting with the Gemini model to support learning-focused development.

## 1. Core Principles for Prompting

-   **Provide Context**: Always state your skill level ("4/10"), your environment ("VS Code Remote Container"), and your goal.
-   **Be Specific**: Ask for exactly what you want (e.g., "a function that reads a CSV," not "process data").
-   **Request Learning Explanations**: Explicitly ask for explanations of Python concepts, patterns, and error messages.
-   **Reference the Interaction Protocol**: All responses should adhere to the standards in `01-interaction-protocol.md`.

## 2. Standard Prompting Templates

### Basic Code Generation
```
I'm a Python beginner (skill level 4/10) working in a VS Code Remote Container at `{PROJECT_ROOT}`.

My task is to [describe what you want to build in simple terms].

Please provide a solution that follows all the rules in our `clinerules`, especially the standard response structure from `01-interaction-protocol.md`.

Ensure the code is simple, includes learning explanations for any new concepts, and provides clear instructions for testing.
```

### Debugging Assistance
```
I need help debugging a Python script. I'm a skill level 4/10.

**My Goal**: [Explain what the code is supposed to do.]

**The Code**:
```python
[Paste your complete, runnable code snippet]
```

**The Error**:
```
[Paste the full, exact error message]
```

Please explain what's causing this error in simple terms, show me the corrected code, and tell me what Python concept I should study to avoid this in the future. Follow the standard response protocol.
```

### Code Improvement
```
I have this working Python code, but I want to learn how to make it better. My skill level is 4/10.

**The Code**:
```python
[Paste your complete, working code snippet]
```

Please refactor this code to improve its readability and add proper error handling, following our project's coding standards.

Explain each change you make and the Python principle behind it. Keep the core logic simple enough for me to understand.
```

## 3. Project-Specific Prompting Patterns

### Web Scraping
```
Create a Python script that scrapes [specific data] from [URL].

I'm learning web scraping (skill level 4/10). Please use the `requests` and `BeautifulSoup` libraries.

The script should:
- Include polite delays.
- Handle common network and parsing errors.
- Save the extracted data to a CSV file in `{PROJECT_ROOT}/data/output/`.

Please follow the standard response protocol, explaining the steps and concepts involved.
```

### Data Analysis
```
Create a Python script to analyze the data in `{PROJECT_ROOT}/data/input/my_data.csv`.

I'm learning data analysis with pandas (skill level 4/10).

The script should:
1. Load the CSV file.
2. Perform basic data cleaning (handle missing values, remove duplicates).
3. Calculate [a specific metric, e.g., the average value of a column].
4. Print a summary of the results.

Explain each `pandas` operation and provide the complete, runnable script.
```

## 4. Multi-Step Workflow Prompting

To build features incrementally, use a series of prompts.

-   **Step 1**: "Let's start by creating a basic function that [does the core task]. Don't worry about error handling yet."
-   **Step 2**: "Great. Now, let's add `try...except` blocks to the previous function to handle potential errors."
-   **Step 3**: "Okay, now let's add logging to that function so we can see what it's doing."

This approach aligns with the "Progressive Complexity" pattern and makes learning more manageable.
