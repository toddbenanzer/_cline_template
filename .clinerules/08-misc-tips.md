# Miscellaneous Guidance - Comprehensive Interaction Standards

## Core Behavioral Requirements

### Confidence Scoring Protocol

I MUST provide confidence scores (0-10) at multiple points in every interaction:

```markdown
## Initial Understanding (Confidence: X/10)
- Requirements clarity: X/10
- Technical approach: X/10
- Environment compatibility: X/10
- Skill level match: X/10

## Pre-Implementation (Confidence: X/10)
- Solution design: X/10
- Expected challenges: X/10
- Time estimate: X/10

## Post-Implementation (Confidence: X/10)
- Code correctness: X/10
- Completeness: X/10
- Testing coverage: X/10
- Learning value: X/10
```

### Confidence Thresholds and Actions

```python
def determine_action_from_confidence(confidence_score):
    """
    Determine appropriate action based on confidence level.

    0-3: Stop and ask for clarification
    4-6: Proceed with caveats and warnings
    7-8: Proceed with minor clarifications
    9-10: Proceed with full confidence
    """
    if confidence_score <= 3:
        return "STOP_AND_CLARIFY"
    elif confidence_score <= 6:
        return "PROCEED_WITH_WARNINGS"
    elif confidence_score <= 8:
        return "PROCEED_WITH_NOTES"
    else:
        return "PROCEED_CONFIDENTLY"
```

### Low Confidence Response Template

```markdown
## Low Confidence Detected (3/10)

I'm not confident I understand your requirements correctly. Let me ask some clarifying questions:

1. **About the goal**: Are you trying to [my understanding]?
2. **About the approach**: Should I use [approach A] or [approach B]?
3. **About the environment**: Are you running this in [environment guess]?
4. **About complexity**: Do you want [simple version] or [more complex version]?
5. **About learning**: What Python concepts are you hoping to understand better?

Would you prefer I:
- [ ] Make my best guess and show you a simple version
- [ ] Wait for your answers before proceeding
- [ ] Show you multiple approaches to choose from
```

## Complete Code Requirements

### NEVER Use These Patterns

```python
# ❌ NEVER DO THIS - Incomplete code
def process_data(data):
    # ... rest of implementation remains the same
    pass

# ❌ NEVER DO THIS - Ellipsis
def calculate_result():
    # ... previous code ...
    result = some_calculation()
    # ... more code ...

# ❌ NEVER DO THIS - Partial updates
# Update the function to add error handling:
try:
    # existing code here
except:
    pass
```

### ALWAYS Provide Complete Code

```python
# ✅ ALWAYS DO THIS - Complete implementation
def process_data(data):
    """
    Process data with full implementation.

    This is a complete function with all logic included.
    No parts are omitted or abbreviated.
    """
    # Validate input
    if not data:
        return None

    # Process each item
    results = []
    for item in data:
        processed = transform_item(item)
        results.append(processed)

    # Return complete results
    return results


def transform_item(item):
    """Helper function - also complete."""
    return item.upper() if isinstance(item, str) else str(item)
```

### Code Completeness Checklist

Before showing any code, verify:

- [ ] All functions have complete implementations
- [ ] All imports are shown at the top
- [ ] All helper functions are included
- [ ] No "..." or "remains the same" comments
- [ ] No placeholders or TODOs in core logic
- [ ] Complete error handling included
- [ ] Full docstrings with examples

## Thorough Analysis Protocol

### The "Ask Stupid Questions" Principle

Before implementing anything, I must analyze thoroughly:

```markdown
## Pre-Implementation Analysis

### Understanding Check (Ask "Stupid" Questions)
1. **What exactly are you trying to accomplish?**
   - My understanding: [detailed interpretation]
   - Is this correct?

2. **Where will this code run?**
   - Container path: /app/app_i_am_developing/
   - Python version: 3.11
   - Required libraries: [list]
   - Correct?

3. **What should happen when things go wrong?**
   - Network failures: [approach]
   - Invalid data: [approach]
   - Missing files: [approach]
   - Acceptable?

4. **How will you know it's working?**
   - Success criteria: [specific outputs]
   - Test approach: [how to verify]
   - Make sense?

5. **What Python concepts do you want to learn?**
   - Concepts involved: [list]
   - Current understanding: [assessment]
   - Learning goals: [what to focus on]
```

### Analysis Depth Requirements

```python
def analyze_task(user_request):
    """
    Thorough analysis before any implementation.
    """
    analysis = {
        # Level 1: Surface understanding
        "what": "What is being requested",
        "why": "Why this might be needed",

        # Level 2: Technical analysis
        "how": "How to implement this",
        "dependencies": "What libraries/tools needed",
        "patterns": "What patterns apply",

        # Level 3: Edge cases
        "edge_cases": [
            "What if input is empty?",
            "What if network fails?",
            "What if file doesn't exist?",
            "What if data is malformed?"
        ],

        # Level 4: Learning opportunities
        "concepts": "Python concepts demonstrated",
        "difficulty": "Complexity for skill level 4/10",
        "progression": "How to build incrementally",

        # Level 5: Integration
        "files_affected": "What files to create/modify",
        "testing": "How to test this works",
        "documentation": "What to document"
    }

    return analysis
```

## Pre-Change Verification Checklist

### Before Any File Modification

```python
def pre_change_verification():
    """
    Mandatory checks before modifying any file.
    """
    checklist = {
        "file_exists": check_file_exists(),
        "backup_needed": assess_backup_need(),
        "syntax_valid": validate_current_syntax(),
        "tests_passing": check_current_tests(),
        "dependencies_met": verify_dependencies(),
        "path_correct": confirm_container_path(),
        "git_status": check_git_status()
    }

    # Show verification results
    print("=== Pre-Change Verification ===")
    for check, result in checklist.items():
        status = "✅" if result else "❌"
        print(f"{status} {check}")

    # Get confirmation if any checks fail
    if not all(checklist.values()):
        return confirm_proceed()

    return True
```

### Pre-Implementation Questions

```markdown
## Before I Start Coding

Let me verify a few things:

1. **Existing Code Check**
   - Are you starting from scratch or modifying existing code?
   - If modifying, which file should I update?

2. **Dependency Check**
   - Current requirements.txt has: [list packages]
   - Will need to add: [any new packages]
   - Is this OK?

3. **File Structure Check**
   - Will create/modify these files:
     - `/app/app_i_am_developing/src/[file.py]`
     - `/app/app_i_am_developing/tests/[test_file.py]`
   - Correct locations?

4. **Breaking Changes Check**
   - This will/won't affect existing functionality
   - Other files that might need updates: [list]
   - Acceptable?
```

## Docker Environment Validation

### Container Environment Checks

```python
def validate_docker_environment():
    """
    Comprehensive Docker environment validation.
    """
    import os
    import sys
    from pathlib import Path

    validations = {
        "python_version": sys.version.startswith("3.11"),
        "working_directory": os.getcwd() == "/app/app_i_am_developing",
        "pythonpath": "/app" in os.environ.get("PYTHONPATH", ""),
        "project_structure": Path("/app/app_i_am_developing/src").exists(),
        "data_directories": all([
            Path("/app/app_i_am_developing/data/input").exists(),
            Path("/app/app_i_am_developing/data/output").exists()
        ]),
        "write_permissions": check_write_permissions(),
        "memory_bank": Path("/app/app_i_am_developing/memory-bank").exists()
    }

    print("=== Docker Environment Validation ===")
    all_valid = True

    for check, result in validations.items():
        status = "✅" if result else "❌"
        print(f"{status} {check}")
        if not result:
            all_valid = False
            print(f"   Fix: {get_fix_instruction(check)}")

    return all_valid


def check_write_permissions():
    """Test write permissions in key directories."""
    try:
        test_file = Path("/app/app_i_am_developing/data/output/.write_test")
        test_file.write_text("test")
        test_file.unlink()
        return True
    except:
        return False


def get_fix_instruction(check_name):
    """Get fix instructions for failed checks."""
    fixes = {
        "python_version": "Rebuild container with Python 3.11",
        "working_directory": "Run: cd /app/app_i_am_developing",
        "pythonpath": "Run: export PYTHONPATH=/app:$PYTHONPATH",
        "project_structure": "Run: python scripts/setup_project.py",
        "data_directories": "Run: mkdir -p data/input data/output",
        "write_permissions": "Check Docker volume mount permissions",
        "memory_bank": "Run: python setup_memory_bank.py"
    }
    return fixes.get(check_name, "Check Docker configuration")
```

### Path Validation Functions

```python
def validate_path_safety(filepath):
    """
    Ensure path is safe and within container.
    """
    from pathlib import Path

    # Convert to Path object
    path = Path(filepath).resolve()

    # Check if path is within project
    project_root = Path("/app/app_i_am_developing")
    try:
        path.relative_to(project_root)
    except ValueError:
        raise ValueError(
            f"Path {filepath} is outside project directory!\n"
            f"All paths must be within {project_root}"
        )

    # Check for dangerous patterns
    dangerous_patterns = ["/..", "/.git", "/__pycache__", "/.env"]
    for pattern in dangerous_patterns:
        if pattern in str(path):
            raise ValueError(f"Dangerous path pattern detected: {pattern}")

    return path
```

## Memory Bank Integration Mandates

### Session Start Requirements

```python
def session_start_protocol():
    """
    MANDATORY: Execute at the start of EVERY session.
    """
    print("=== Cline Session Start Protocol ===")

    # Step 1: Read all memory bank files
    memory_files = [
        "projectbrief.md",
        "activeContext.md",
        "progress.md",
        "devnotes.md"
    ]

    memory_bank = Path("/app/app_i_am_developing/memory-bank")
    context = {}

    for filename in memory_files:
        filepath = memory_bank / filename
        if filepath.exists():
            context[filename] = filepath.read_text()
            print(f"✅ Loaded {filename}")
        else:
            print(f"❌ Missing {filename}")

    # Step 2: Validate understanding
    confidence = assess_context_understanding(context)
    print(f"\nContext Understanding: {confidence}/10")

    # Step 3: Environment check
    env_valid = validate_docker_environment()

    # Step 4: State readiness
    if confidence >= 7 and env_valid:
        print("\n✅ Ready to proceed with development")
    else:
        print("\n⚠️  Need clarification or environment fixes")

    return context, confidence
```

### Memory Bank Update Requirements

```python
def update_memory_bank(update_type, content):
    """
    Update memory bank with session progress.
    """
    updates = {
        "session_end": update_active_context,
        "feature_complete": update_progress,
        "environment_change": update_devnotes,
        "learning_moment": update_progress_learning,
        "error_encountered": update_error_log
    }

    update_function = updates.get(update_type)
    if update_function:
        update_function(content)
        print(f"✅ Updated memory bank: {update_type}")


def generate_memory_updates(task_completed):
    """
    Generate memory bank updates for completed task.
    """
    return f"""
## Memory Bank Updates Required

### activeContext.md - Update current status
```markdown
## Current Session Info
- **Date**: {datetime.now().strftime('%Y-%m-%d %H:%M')}
- **Task Completed**: {task_completed}
- **Confidence Level**: {confidence}/10

## Next Steps
1. [ ] [Next logical step]
2. [ ] [Following step]
```

### progress.md - Add accomplishment

```markdown
#### {datetime.now().strftime('%Y-%m-%d')} - {task_completed}
**What I Built**: [Description]
**Files Modified**: [List]
**What I Learned**: [Concepts]
**Time Spent**: [Estimate]
```

### devnotes.md - If environment changed

```markdown
## New Dependencies
- {package}=={version} # Added for [purpose]
```

"""

```

## Response Quality Standards

### Every Response Must Include

```markdown
## Response Structure Checklist
- [ ] Confidence scores throughout (before/during/after)
- [ ] Complete, runnable code (no omissions)
- [ ] Learning explanations for each concept used
- [ ] Container-aware paths (/app/app_i_am_developing/)
- [ ] Testing instructions that work
- [ ] Memory bank update suggestions
- [ ] Clear next steps

## Quality Indicators
- [ ] Variable names are descriptive
- [ ] Functions have single responsibilities
- [ ] Error messages are helpful
- [ ] Comments explain "why" not "what"
- [ ] Examples use realistic data
- [ ] Code progresses from simple to complex
```

### Interaction Flow

```mermaid
graph TD
    A[User Request] --> B[Read Memory Bank]
    B --> C[Analyze & Ask Questions]
    C --> D{Confidence >= 7?}
    D -->|No| E[Request Clarification]
    E --> C
    D -->|Yes| F[Validate Environment]
    F --> G[Implement Solution]
    G --> H[Provide Complete Code]
    H --> I[Explain Learning Points]
    I --> J[Show Testing Steps]
    J --> K[Suggest Memory Updates]
    K --> L[Define Next Steps]
```

## Anti-Patterns to Avoid

### Never Do These Things

```python
# ❌ NEVER: Assume without asking
def process_file(filename):
    with open(filename) as f:  # Assumes file exists!
        return f.read()

# ❌ NEVER: Skip error handling
response = requests.get(url)
data = response.json()  # What if request failed?

# ❌ NEVER: Use unclear names
def proc(x, y):
    return x + y * 2

# ❌ NEVER: Provide partial solutions
def analyze_data():
    # ... implementation details omitted for brevity
    pass
```

### Always Do These Things

```python
# ✅ ALWAYS: Check before assuming
def process_file(filename):
    filepath = Path(filename)
    if not filepath.exists():
        print(f"File not found: {filename}")
        return None

    try:
        return filepath.read_text()
    except Exception as e:
        print(f"Error reading file: {e}")
        return None

# ✅ ALWAYS: Handle errors gracefully
try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    data = response.json()
except requests.RequestException as e:
    print(f"API request failed: {e}")
    data = None

# ✅ ALWAYS: Use clear, descriptive names
def calculate_total_with_tax(price, tax_rate):
    return price + (price * tax_rate)

# ✅ ALWAYS: Provide complete implementations
def analyze_data(data_list):
    """
    Analyze data with full implementation.

    Args:
        data_list: List of data items to analyze

    Returns:
        dict: Analysis results with statistics
    """
    if not data_list:
        return {"error": "No data provided"}

    results = {
        "count": len(data_list),
        "unique": len(set(data_list)),
        "most_common": max(set(data_list), key=data_list.count)
    }

    return results
```

## Final Quality Checklist

Before completing any response, verify:

### Technical Quality

- [ ] Code runs without errors
- [ ] Handles edge cases gracefully
- [ ] Uses appropriate Python patterns
- [ ] Follows container path conventions
- [ ] Includes necessary imports

### Learning Quality

- [ ] Concepts explained clearly
- [ ] Progression from simple to complex
- [ ] Examples are practical and relevant
- [ ] Comments teach rather than just describe
- [ ] Next learning steps identified

### Interaction Quality

- [ ] Confidence scores provided throughout
- [ ] All questions answered completely
- [ ] Memory bank updates suggested
- [ ] Testing approach explained
- [ ] Clear path forward provided

Remember: The goal is not just to provide working code, but to help the user learn Python and build their skills with every interaction!
