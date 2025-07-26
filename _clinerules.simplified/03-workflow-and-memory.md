# Workflow and Memory Management

## Memory Bank Structure

Four core files in `{PROJECT_ROOT}/memory-bank/`:

- **`projectbrief.md`**: *(Read-Only during session)* High-level project goals and learning objectives
- **`activeContext.md`**: *(Read-Write)* Short-term memory for current session - immediate tasks, recent decisions, next steps
- **`progress.md`**: *(Append-Only)* Long-term memory - completed features, skills learned, key code snippets
- **`devnotes.md`**: *(Append-Only)* Technical details - dependency changes, environment setup, debugging solutions

## Session Workflow: Read → Act → Update

**MANDATORY** three-step cycle for every development session:

### Step 1: Read Memory Bank (Session Start)
- Read all four memory bank files at session beginning
- State confidence in understanding context
- Essential for understanding project state and immediate goals

### Step 2: Act on Task
- Proceed with development keeping memory bank context in mind
- Reference `projectbrief.md` for goals and `progress.md` for established patterns

### Step 3: Update Memory Bank (Session End)
- Provide pre-formatted "Memory Bank Update" blocks for meaningful changes
- Allows easy copy-paste updates to correct files

## Update Triggers

**Update when:**
- **`activeContext.md`**: End of every session with current status and next steps
- **`progress.md`**: Feature completed or new concept learned
- **`devnotes.md`**: New dependency added, configuration changed, or tricky bug solved

## Project Workflows

Follow structured, step-by-step workflows for common project types:

- **Web Scraping**: Exploration → Prototyping → Building (Make it Work → Safe → Flexible)
- **Data Analysis**: Exploration → Cleaning Pipeline → Analysis/Visualization
- **Automation Scripts**: Define Task → Build Core Logic → Add Safety/Configuration

Each workflow follows the Progressive Complexity Pattern and emphasizes learning through building real solutions.
