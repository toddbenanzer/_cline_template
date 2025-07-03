# Google Gemini Optimization for Personal Python Development

## Memory Bank Maintenance Prompts

### Session Start Prompt
```
"Follow your custom instructions and read all memory bank files.

Current focus: [what you want to work on today]
Container status: [running/stopped]
Last session progress: [brief recap if you remember]

Please summarize what you understand about:
1. Current project status
2. What I was last working on
3. Immediate next steps
4. Any environment issues to be aware of"
```

### End of Session Update
```
"Update memory bank with today's progress:

What I accomplished:
- [Specific features/functions completed]
- [Problems solved]
- [Files modified]

What I learned:
- [New Python concepts]
- [Docker/environment insights]
- [Development workflow improvements]

Current status:
- [What's working now]
- [What needs work next]
- [Any blockers or questions]

Next session goals:
- [Immediate next step]
- [What to focus on next time]

Please update the appropriate memory bank files with this information."
```

### Problem-Solving Session Update
```
"I solved a [problem type] today. Update memory bank with:

Problem: [What wasn't working]
Solution: [How I fixed it]
Key learnings: [What I learned from this]
Prevention: [How to avoid this in the future]
Files affected: [Which files I changed]

Add this to progress.md and devnotes.md as appropriate."
```

### Weekly Memory Bank Review
```
"Review my memory bank files for accuracy and relevance:

1. Check activeContext.md - remove outdated information
2. Verify current project goals still match my actual goals
3. Clean up old progress entries that aren't relevant
4. Update development environment notes if anything changed
5. Highlight any inconsistencies or missing information

Suggest what needs updating to keep the memory bank useful."
```

### Optimal Configuration
```json
{
    "model": "gemini-2.0-flash-experimental",
    "temperature": 0.2,
    "max_output_tokens": 800
}
```

**Why these settings work for you:**
- **Lower temperature (0.2)**: More consistent, readable code generation
- **Moderate token limit**: Focused responses without overwhelming complexity
- **Flash model**: Fast responses for interactive development

## Prompting for Your Development Environment

### Context Setting for Docker + WSL2
```
"I'm developing in VS Code with Docker and WSL2. My project structure is:
- development_folder/ (root with Docker files)
- app_i_am_developing/ (application code)
- Standard folders: src/, examples/, tests/, config/

I prefer simple, readable code over complex solutions. Please keep responses focused on my personal-use Python applications."
```

### Effective Prompts for Personal Development

#### Project Structure Setup
```
"Help me set up a new Python project in my standard structure:
- Docker development environment
- Basic folder structure with src/, examples/, tests/, config/
- Simple requirements.txt for personal use
- Keep it minimal and focused on readability"
```

#### Code Generation Requests
```
"I need a Python function that [specific task].

Requirements:
- Simple and readable code
- Minimal error handling (only what's necessary)
- Clear variable names
- Works in my Docker environment
- Include a simple example of how to use it

My skill level: intermediate beginner (4/10)"
```

#### Docker-Specific Prompts
```
"I'm working in a Docker container with VS Code. Help me:
- [specific task]
- Make sure it works with mounted volumes
- Keep file paths relative to /app
- Use simple Python patterns I can understand and maintain"
```

## Learning-Focused Prompting

### Progressive Development Requests
```
"I want to build [feature] step by step:

Step 1: Show me the simplest version that works
Step 2: Add basic error handling
Step 3: Make it more reusable

Please explain each step and why you made those choices. Keep the code readable over clever."
```

### Code Review and Improvement
```
"Review this code I wrote for readability and simplicity:

[paste your code]

Please suggest improvements that:
- Make it more readable
- Remove unnecessary complexity
- Follow Python best practices for personal projects
- Keep it maintainable for someone at my skill level"
```

### Problem-Solving Pattern
```
"I'm trying to [specific goal] in my Python application.

My current approach:
[describe what you're doing]

Challenges I'm facing:
[specific problems]

Please suggest a simple, readable solution that:
- Works in my Docker environment
- Uses standard Python libraries
- Is easy to understand and modify
- Follows my preference for minimal complexity"
```

## Docker Development Prompts

### Container Setup Help
```
"Help me set up my Docker development environment:

Current setup: VS Code + Docker + WSL2
Project structure: [describe your structure]

I need:
- Simple Dockerfile for Python development
- docker-compose.yml for development
- Volume mounting for my code
- Basic Python environment setup

Keep it simple and well-commented."
```

### File Operations in Docker
```
"I need to process files in my Docker container:
- Files are in /app/app_i_am_developing/data/
- I want to [specific file operation]
- Keep paths relative and portable
- Handle the case where files don't exist simply

Show me a straightforward approach."
```

### Environment Configuration
```
"Help me manage configuration in my Docker environment:
- Environment variables for development
- Config files in my config/ folder
- Simple settings that work in containers
- Easy to modify for different environments

Focus on simplicity and readability."
```

## Code Quality Prompts

### Readability Focus
```
"Make this code more readable and maintainable:

[paste code]

Priorities:
1. Clear variable names
2. Simple function structure
3. Minimal but appropriate error handling
4. Easy to understand logic flow
5. Good comments where needed

Don't make it more complex - make it clearer."
```

### Simplification Requests
```
"This code works but feels too complex for my needs:

[paste code]

Can you simplify it by:
- Removing unnecessary abstractions
- Using more straightforward logic
- Reducing the number of functions/classes
- Making it easier to debug and modify

I prefer simple over elegant."
```

## Testing and Validation

### Simple Testing Help
```
"Help me write simple tests for this function:

[paste function]

I want:
- Basic assert statements
- Test with realistic data
- Easy to run and understand
- Don't overcomplicate it with testing frameworks

Show me a simple test that validates it works."
```

### Manual Testing Approaches
```
"I want to test this code manually before automating:

[paste code]

Suggest:
- Simple test cases to try
- Sample data to use
- What to check for
- How to verify it's working correctly

Keep the testing approach simple and practical."
```

## Debugging and Troubleshooting

### Error Understanding
```
"I'm getting this error in my Docker container:

Error: [paste error]

My code:
[paste relevant code]

Environment: VS Code + Docker + WSL2

Please:
1. Explain what this error means in simple terms
2. Show me how to fix it
3. Suggest how to prevent it in the future
4. Keep the solution simple and readable"
```

### Docker-Specific Issues
```
"I'm having issues with [specific problem] in my Docker development setup:

Setup: [describe your setup]
Problem: [describe the issue]
What I've tried: [what you've attempted]

Please suggest a simple solution that works with:
- Docker volumes
- WSL2 file system
- VS Code integration
- My project structure"
```

## Incremental Development

### Feature Addition
```
"I have a working Python module and want to add [new feature]:

Current working code:
[paste code]

New feature requirements:
- [specific requirement]
- Keep it simple and readable
- Don't break existing functionality
- Easy to test and verify

Show me how to add this incrementally."
```

### Refactoring Requests
```
"My code is getting messy. Help me refactor for better organization:

Current code:
[paste code]

Goals:
- Better file/function organization
- Clearer separation of concerns
- Easier to understand and maintain
- Still simple and straightforward

Suggest a clean, readable structure."
```

## Learning and Skill Development

### Concept Learning
```
"I want to understand [Python concept] better:

My current understanding: [what you know]
What I'm trying to do: [your goal]
Where I'm confused: [specific confusion]

Please explain:
- The concept in simple terms
- When and why to use it
- A practical example I can try
- How it fits with my current skill level"
```

### Best Practices Questions
```
"For my personal Python projects, what are the best practices for:
- [specific area like file organization, error handling, etc.]

Consider:
- My skill level (4/10)
- Personal use (not enterprise)
- Docker development environment
- Focus on readability and maintainability

Give me practical, actionable advice."
```

## Response Format Requests

### Structured Learning Response
```
"Please structure your response like this:

## Simple Solution
[the basic code that works]

## How It Works
[step-by-step explanation]

## Why This Approach
[reasoning behind the choices]

## Usage Example
[practical example I can try]

## Next Steps
[what to try next or improve]"
```

### Code-Heavy Response
```
"Focus on showing me working code with:
- Inline comments explaining key parts
- Simple structure I can understand
- Realistic example data
- How to integrate with my project structure

Minimize explanatory text - let the code speak."
```

## Common Scenarios

### Data Processing Tasks
```
"I need to process [type of data] in my personal project:
- Input: [describe input]
- Output: [describe desired output]
- Constraints: Simple, readable, works in Docker
- My skill level: intermediate beginner

Show me a straightforward approach with examples."
```

### File Management Tasks
```
"Help me organize/process files in my application:
- Task: [specific file operation]
- Location: app_i_am_developing/data/
- Requirements: Simple, clear, minimal error handling
- Environment: Docker container

Provide a clean, readable solution."
```

Remember: I prefer simple, readable solutions over complex, elegant ones. Focus on helping me understand and maintain the code rather than showing off advanced techniques!
