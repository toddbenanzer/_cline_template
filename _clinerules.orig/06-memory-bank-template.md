# Memory Bank Templates

## Project Brief Template (projectbrief.md)

```markdown
# Project Brief: [Project Name]

## Overview
[One paragraph description of what this project does and why I'm building it]

## Learning Objectives
- [ ] Learn [Python concept 1] through [specific feature]
- [ ] Understand [Python concept 2] by implementing [component]
- [ ] Practice [skill] while building [feature]
- [ ] Master [technique] for [use case]

## Core Requirements
### Must Have (MVP)
- [ ] [Basic feature 1 that makes it useful]
- [ ] [Basic feature 2 for core functionality]
- [ ] [Basic feature 3 for minimum viability]

### Nice to Have (Future)
- [ ] [Enhanced feature 1]
- [ ] [Enhanced feature 2]
- [ ] [Advanced feature]

## Technical Specifications
- **Language**: Python 3.11+
- **Environment**: VS Code Remote Container
- **Key Libraries**:
  - [Library 1] for [purpose]
  - [Library 2] for [purpose]
- **Data Sources**: [Where data comes from]
- **Output Format**: [What the project produces]

## Success Criteria
This project will be successful when:
1. I can [specific user action] and get [specific result]
2. The code is readable enough that I can understand it in 3 months
3. I've learned [specific Python concepts] well enough to use them again
4. It solves my actual problem of [real-world issue]

## Constraints and Assumptions
- **Time**: Aiming for [X hours/days] of development
- **Complexity**: Keeping it simple enough for skill level 4/10
- **Scope**: Personal use only, not production-ready
- **Performance**: Good enough for [expected data size/frequency]

## Example Use Case
```

[Show a concrete example of how I'll use this project]
Input: [example input]
Process: [what happens]
Output: [example output]

```

## Notes
- [Any special considerations]
- [Lessons learned from similar projects]
- [Resources or references to check]

---
Created: [Date]
Last Updated: [Date]
```

## Active Context Template (activeContext.md)

```markdown
# Active Context

## Current Session Info
- **Date**: [Today's date]
- **Session Start**: [Time]
- **Mode**: [development/debugging/learning/refactoring]
- **Confidence Level**: [X/10] in current task

## What I'm Working On
### Current Task
[Specific feature or problem I'm solving right now]

### Why This Task
[How it connects to the larger project goals]

### Approach
[My plan for implementing this]

## Recent Decisions
1. **Decision**: [What I decided]
   **Reason**: [Why I made this choice]
   **Alternative**: [What I considered but didn't choose]

2. **Decision**: [What I decided]
   **Reason**: [Why I made this choice]
   **Alternative**: [What I considered but didn't choose]

## Current Challenges
- **Challenge**: [What's difficult]
  **Attempted**: [What I've tried]
  **Next Attempt**: [What to try next]

## Code Locations
- Working on: `/app/app_i_am_developing/src/[current_file.py]`
- Related files:
  - `[file1.py]` - [what it does]
  - `[file2.py]` - [what it does]

## Terminal Commands Used
```bash
# Recent useful commands
[command 1]  # What it did
[command 2]  # What it did
```

## Learning Notes

- **Concept**: [Python concept encountered]
  **Understanding**: [What I learned about it]
  **Example**: [How I used it]

## Next Steps (Priority Order)

1. [ ] [Immediate next action]
2. [ ] [Following action]
3. [ ] [After that]

## Questions for Next Session

- [ ] How do I [specific technical question]?
- [ ] Should I [architectural decision]?
- [ ] Is there a better way to [current approach]?

## Session End Notes

- **Progress Made**: [What I accomplished]
- **Confidence Now**: [X/10]
- **Ready for Next Time**: [Yes/No - what needs setup]

---
Session End: [Time]
Total Time: [Duration]

```

## Progress Template (progress.md)

```markdown
# Development Progress Log

## Project Timeline

### Week of [Date]

#### [Date] - [Feature/Task Name]
**What I Built**:
[Description of what was implemented]

**Files Created/Modified**:
- `src/[file.py]` - [what changed]
- `tests/[test_file.py]` - [tests added]

**What Worked**:
```python
# Code snippet showing successful implementation
def successful_function():
    """This approach worked well"""
    pass
```

**What I Learned**:

- **Concept**: [Python concept]
  **How It Works**: [Explanation]
  **When To Use**: [Use cases]

**Challenges Overcome**:

- **Problem**: [What went wrong]
  **Solution**: [How I fixed it]
  **Lesson**: [What to remember]

**Time Spent**: [X hours]
**Confidence Gained**: [Before X/10 → After Y/10]

---

### Week of [Previous Date]

#### [Date] - [Feature/Task Name]

[Similar structure as above]

## Completed Features

### ✅ Feature: [Feature Name]

- **Completed**: [Date]
- **Purpose**: [What it does]
- **Usage**: `python src/feature.py --example`
- **Key Learning**: [Main concept mastered]

### ✅ Feature: [Feature Name]

[Similar structure]

## Skills Progression

### Python Concepts Learned

1. **File I/O Operations**
   - Level: ⭐⭐⭐☆☆
   - Can: Read/write files, handle errors
   - Used in: Data processing scripts

2. **API Integration**
   - Level: ⭐⭐☆☆☆
   - Can: Make requests, parse JSON
   - Used in: LLM integration, web scraping

3. **Error Handling**
   - Level: ⭐⭐⭐☆☆
   - Can: Try/except, custom messages
   - Used in: All modules

[Continue list]

## Code Snippets to Remember

### Pattern: Safe File Reading

```python
def read_file_safely(filepath):
    """Pattern I use often"""
    path = Path(filepath)
    if not path.exists():
        return None
    try:
        return path.read_text()
    except Exception as e:
        print(f"Error: {e}")
        return None
```

### Pattern: API Call with Retry

```python
def api_call_with_retry(url, max_retries=3):
    """Reliable API calling pattern"""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()
            return response.json()
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)
```

## Milestones Reached

### 🎯 Milestone: First Working Prototype

- **Date**: [Date]
- **Achievement**: [What it could do]
- **Celebration**: [How I celebrated]

### 🎯 Milestone: [Milestone Name]

[Similar structure]

## Ideas for Future

### Enhancement Ideas

1. **Idea**: [Enhancement description]
   **Benefit**: [Why it would help]
   **Complexity**: [Easy/Medium/Hard]
   **Prerequisites**: [What I need to learn first]

### New Project Ideas Inspired by This

1. [Project idea based on learnings]
2. [Another project idea]

---
Last Updated: [Date]
Total Development Hours: [X]

```

## Dev Notes Template (devnotes.md)

```markdown
# Development Notes

## Environment Setup

### Docker Configuration
```dockerfile
# Current Dockerfile setup
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
# Add any custom configurations
```

### VS Code Settings

```json
{
    "python.defaultInterpreterPath": "/usr/local/bin/python",
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true,
    "python.formatting.provider": "black",
    "editor.formatOnSave": true,
    "files.autoSave": "afterDelay"
}
```

### Container Startup Commands

```bash
# Standard startup sequence
cd /app/app_i_am_developing
pip install -r requirements.txt
python --version
# Any other setup commands
```

## Dependencies

### Current requirements.txt

```
# Core dependencies
python-dotenv==1.0.0
requests==2.31.0
pandas==2.0.3
beautifulsoup4==4.12.2

# LLM APIs
google-generativeai==0.3.0
openai==1.3.0  # if using

# Web framework (if applicable)
flask==3.0.0

# Testing
pytest==7.4.0

# Data science (if applicable)
matplotlib==3.7.1
numpy==1.24.3

# Add new dependencies here with date and reason
# [Date] - added [package] for [purpose]
```

### Installation Issues & Solutions

1. **Package**: [package name]
   **Issue**: [What went wrong]
   **Solution**: [How to fix]

   ```bash
   # Commands that fixed it
   ```

## Common Issues & Solutions

### Issue: Import Error in Container

**Symptom**: `ModuleNotFoundError` even after pip install
**Cause**: Python path not set correctly
**Solution**:

```bash
export PYTHONPATH=/app:$PYTHONPATH
# Or add to Dockerfile
ENV PYTHONPATH=/app:$PYTHONPATH
```

### Issue: File Not Found in Container

**Symptom**: Works locally but not in container
**Cause**: Path differences between local and container
**Solution**: Always use absolute paths from `/app/app_i_am_developing/`

### Issue: [Common Issue]

**Symptom**: [What you see]
**Cause**: [Why it happens]
**Solution**: [How to fix]

## Debugging Techniques That Work

### For Web Scraping Issues

```python
# Save HTML for offline debugging
def debug_scraping():
    response = requests.get(url)
    with open('/app/app_i_am_developing/debug/page.html', 'w') as f:
        f.write(response.text)
    # Now can test parsing without hitting site
```

### For Data Processing Issues

```python
# Check data at each step
def debug_pipeline(df):
    print(f"Step 1 - Shape: {df.shape}")
    print(f"Step 1 - Columns: {df.columns.tolist()}")
    print(f"Step 1 - Sample:\n{df.head()}")
    # Continue for each transformation
```

### For API Issues

```python
# Log full request/response
def debug_api_call(url, params):
    print(f"URL: {url}")
    print(f"Params: {params}")
    response = requests.get(url, params=params)
    print(f"Status: {response.status_code}")
    print(f"Headers: {dict(response.headers)}")
    print(f"Response: {response.text[:500]}")
```

## Performance Optimizations

### What Made Things Faster

1. **Optimization**: Changed from [old way] to [new way]
   **Speed Up**: [X]x faster
   **Code**:

   ```python
   # Old way
   [old code]

   # New way
   [new code]
   ```

## Useful Resources

### Documentation Links

- [Python Official Docs](https://docs.python.org/3/)
- [Library Specific]:
  - Pandas: [link]
  - Requests: [link]
  - BeautifulSoup: [link]

### Tutorials That Helped

1. [Tutorial Name] - [What it taught me]
2. [Tutorial Name] - [What it taught me]

### Stack Overflow Solutions

1. [Problem] - [SO link] - [Key insight]
2. [Problem] - [SO link] - [Key insight]

## Git Commands I Use

### Daily Workflow

```bash
# Start work
git pull
git checkout -b feature/new-feature

# Save progress
git add .
git commit -m "Add: [what I added]"

# Learning commits
git commit -m "Learn: [concept] - [what I learned]"

# End of day
git push origin feature/new-feature
```

### Useful Git Aliases

```bash
# Add to ~/.gitconfig
[alias]
    st = status
    co = checkout
    br = branch
    save = !git add -A && git commit -m 'SAVEPOINT'
    undo = reset HEAD~1 --mixed
```

## Terminal Shortcuts

### Frequently Used Commands

```bash
# Navigate to project
alias proj="cd /app/app_i_am_developing"

# Run tests
alias test="python -m pytest tests/ -v"

# Check code
alias check="python scripts/quality_check.py"

# Interactive Python with project imports
alias ipy="cd /app/app_i_am_developing && python -i"
```

## Future Learning Goals

### Next Python Concepts to Learn

1. [ ] Async/await for concurrent operations
2. [ ] Decorators for cleaner code
3. [ ] Context managers for resource handling
4. [ ] Type hints for better code clarity

### Tools to Explore

1. [ ] Poetry for dependency management
2. [ ] Black for code formatting
3. [ ] Ruff for fast linting
4. [ ] Rich for better terminal output

---
Last Updated: [Date]

```

## Quick Setup Script

Save this as `setup_memory_bank.py` in your project root:

```python
#!/usr/bin/env python3
"""
Set up memory bank structure for a new project.
"""

from pathlib import Path
from datetime import datetime


def create_memory_bank():
    """Create memory bank structure with templates."""
    base_path = Path("/app/app_i_am_developing")
    memory_bank = base_path / "memory-bank"

    # Create directory
    memory_bank.mkdir(exist_ok=True)

    # Define templates (simplified versions)
    templates = {
        "projectbrief.md": f"""# Project Brief: My Project

## Overview
[Describe your project here]

## Learning Objectives
- [ ] Learn Python basics through practical application
- [ ] Build something useful for personal use

## Core Requirements
- [ ] Basic functionality that works
- [ ] Simple error handling
- [ ] Clear, readable code

Created: {datetime.now().strftime('%Y-%m-%d')}
""",

        "activeContext.md": f"""# Active Context

## Current Session Info
- **Date**: {datetime.now().strftime('%Y-%m-%d')}
- **Session Start**: {datetime.now().strftime('%H:%M')}
- **Confidence Level**: 0/10 (just starting)

## What I'm Working On
Initial project setup

## Next Steps
1. [ ] Read all memory bank files
2. [ ] Define project goals
3. [ ] Start with simplest feature

Session End: [Time]
""",

        "progress.md": """# Development Progress Log

## Project Timeline

### Week of [Date]
No progress yet - just starting!

## Skills Progression
Starting from skill level 4/10

---
Last Updated: [Date]
""",

        "devnotes.md": f"""# Development Notes

## Environment Setup
- Docker: ✓
- VS Code Remote Container: ✓
- Python 3.11: ✓

## Dependencies
```

# requirements.txt

python-dotenv
requests

```

Created: {datetime.now().strftime('%Y-%m-%d')}
"""
    }

    # Create files
    for filename, content in templates.items():
        filepath = memory_bank / filename
        filepath.write_text(content)
        print(f"✅ Created {filepath}")

    print(f"\n🎉 Memory bank initialized at {memory_bank}")
    print("   Next step: Edit projectbrief.md with your project details")


if __name__ == "__main__":
    create_memory_bank()
```

Run this script to initialize your memory bank:

```bash
python setup_memory_bank.py
```

These templates provide a complete structure for maintaining project context across sessions, tracking your learning progress, and building your Python skills incrementally!
