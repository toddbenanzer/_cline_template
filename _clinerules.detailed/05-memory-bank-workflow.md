# Memory Bank Workflow

The Memory Bank is a critical system for maintaining context and tracking learning progress between sessions. This document defines the mandatory workflow for its use.

## 1. Memory Bank Structure

The memory bank consists of four core files located in the `{PROJECT_ROOT}/memory-bank/` directory.

-   **`projectbrief.md`**: **(Read-Only during session)** Defines the high-level project goals and learning objectives. Should only be modified when project goals change.
-   **`activeContext.md`**: **(Read-Write)** The "short-term memory" for the current session. It tracks the immediate task, recent decisions, and next steps.
-   **`progress.md`**: **(Append-Only)** The "long-term memory." It's a log of completed features, skills learned, and key code snippets.
-   **`devnotes.md`**: **(Append-Only)** A log of technical details, such as dependency changes, environment setup commands, and debugging solutions.

## 2. The Session Workflow: Read -> Act -> Update

Every development session **MUST** follow this three-step cycle.

### Step 1: Read the Memory Bank (Session Start)
At the very beginning of every session, you **MUST** read the contents of all four memory bank files. This is essential for understanding the project's current state, recent progress, and immediate goals. After reading, you must state your confidence in understanding the context.

### Step 2: Act on the Task
Proceed with the development task, keeping the context from the memory bank in mind. Reference the `projectbrief.md` for goals and the `progress.md` for patterns we've used before.

### Step 3: Update the Memory Bank (Session End)
At the end of every response where a meaningful change was made, you **MUST** provide a pre-formatted "Memory Bank Update" block. This allows the user to easily copy and paste the update into the correct file.

**Triggers for Updates:**
-   **`activeContext.md`**: Update at the end of every session with the current status and next steps.
-   **`progress.md`**: Update when a feature is completed or a new concept is learned.
-   **`devnotes.md`**: Update when a new dependency is added, a configuration is changed, or a tricky bug is solved.

## 3. Memory Bank Update Templates

Use these templates to structure the memory bank updates.

### `activeContext.md` Update Block

```markdown
### Memory Bank Update: `activeContext.md`

- **Last Task**: [Brief description of what was just completed]
- **Next Steps**:
  1. [ ] [The next immediate, logical action]
  2. [ ] [The step after that]
- **Blockers**: [Any issues preventing progress, or "None"]
```

### `progress.md` Update Block

```markdown
### Memory Bank Update: `progress.md`

#### [Date] - [Feature or Concept Learned]
**What Was Built**: [Description of the feature or implementation.]
**What I Learned**:
- **Concept**: [Name of the Python concept]
- **How it Works**: [Simple explanation]
**Code Snippet**:
```python
# The key piece of code that demonstrates the learning
[code snippet]
```
```

### `devnotes.md` Update Block

```markdown
### Memory Bank Update: `devnotes.md`

**[Date] - [Type of Note]**
- **Change**: [e.g., Added `requests==2.28.1` to `requirements.txt`]
- **Reason**: [e.g., "Needed for making HTTP API calls."]
```

By strictly following this workflow, we ensure that context is never lost and that every session builds upon the last, creating a powerful, continuous learning loop.
