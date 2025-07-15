name: "Project Setup and Initialization PRP"
description: |

## Purpose
Create essential project structure and documentation files for the Context Engineering template, ensuring all referenced files in CLAUDE.md exist and provide proper guidance for AI coding assistants.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Set up the foundational project structure with PLANNING.md and TASK.md files that are referenced in CLAUDE.md but currently missing. These files will provide essential context for AI assistants working on the project.

## Why
- CLAUDE.md references these files as critical for project awareness
- AI assistants need structured planning and task tracking
- Ensures consistency across all development sessions
- Provides a single source of truth for project architecture and progress

## What
Create comprehensive project documentation that includes:
- PLANNING.md: Project architecture, goals, style guides, and constraints
- TASK.md: Active task tracking with dates and status
- Validation that all CLAUDE.md references work correctly

### Success Criteria
- [ ] PLANNING.md exists with complete project context
- [ ] TASK.md exists with proper task tracking structure
- [ ] All CLAUDE.md file references are valid
- [ ] Documentation follows existing project patterns

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: CLAUDE.md
  why: Contains references to PLANNING.md and TASK.md that need to be created
  
- file: README.md
  why: Understand project purpose and structure to inform PLANNING.md content
  
- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Example of comprehensive documentation style to follow

- file: .claude/commands/generate-prp.md
  why: Understand how PRPs are generated to align documentation

- file: .claude/commands/execute-prp.md
  why: Understand execution process to inform task tracking structure
```

### Current Codebase tree
```bash
context-engineering-intro/
├── .claude/
│   ├── commands/
│   │   ├── execute-parallel.md
│   │   ├── execute-prp.md
│   │   ├── fix-github-issue.md
│   │   ├── generate-prp.md
│   │   └── prep-parallel.md
│   └── settings.local.json
├── .devcontainer/
├── PRPs/
│   ├── templates/
│   │   └── prp_base.md
│   └── EXAMPLE_multi_agent_prp.md
├── examples/
├── CLAUDE.md
├── INITIAL.md
├── INITIAL_EXAMPLE.md
└── README.md
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── .claude/                    # (existing)
├── .devcontainer/             # (existing)
├── PRPs/                      # (existing)
├── examples/                  # (existing)
├── CLAUDE.md                  # (existing)
├── INITIAL.md                 # (existing)
├── INITIAL_EXAMPLE.md         # (existing)
├── README.md                  # (existing)
├── PLANNING.md                # NEW - Project architecture and guidelines
└── TASK.md                    # NEW - Active task tracking

```

### Known Gotchas
```python
# CRITICAL: CLAUDE.md expects specific sections in PLANNING.md
# Must include: architecture, goals, style, constraints

# CRITICAL: TASK.md must support date tracking
# Format should be clear for both reading and updating

# CRITICAL: Must follow existing markdown patterns
# Use consistent heading levels and formatting from other .md files
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create PLANNING.md:
CREATE PLANNING.md:
  - MIRROR structure from: Well-structured project documentation
  - INCLUDE: Project overview, architecture, conventions
  - REFERENCE: Content from README.md for project understanding
  - ENSURE: All sections mentioned in CLAUDE.md are covered

Task 2 - Create TASK.md:
CREATE TASK.md:
  - CREATE: Task tracking structure with dates
  - INCLUDE: Active tasks, completed tasks, discovered tasks
  - PATTERN: Simple, scannable format for AI assistants
  - SUPPORT: Easy updates and status tracking

Task 3 - Validate Integration:
VERIFY all files:
  - CHECK: CLAUDE.md references work correctly
  - TEST: Files are readable and well-formatted
  - ENSURE: No broken references or missing sections
```

### Per task pseudocode

```python
# Task 1 - PLANNING.md Structure
"""
# Project Planning

## Project Overview
[Context Engineering purpose and goals from README.md]

## Architecture
### Directory Structure
[Explain each directory's purpose]

### Core Components
1. PRP System
   - Generation (generate-prp.md)
   - Execution (execute-prp.md)
   - Templates (prp_base.md)

2. Context Management
   - CLAUDE.md (global rules)
   - Examples folder (patterns)
   - INITIAL.md (feature requests)

## Goals
1. Enable one-pass AI implementation
2. Reduce context failures
3. Ensure consistency

## Style Guidelines
### Documentation
- Use clear markdown formatting
- Include examples liberally
- Reference specific files

### Code Patterns
- Follow examples/ patterns
- Use type hints
- Include comprehensive docstrings

## Constraints
- Keep context focused and relevant
- Validate continuously
- Follow CLAUDE.md rules strictly
"""

# Task 2 - TASK.md Structure
"""
# Task Tracking

## Active Tasks

### Task: [Task Name]
- **Date Added**: YYYY-MM-DD
- **Status**: In Progress
- **Description**: [What needs to be done]
- **Notes**: [Any relevant context]

## Completed Tasks

### Task: [Completed Task Name]
- **Date Added**: YYYY-MM-DD
- **Date Completed**: YYYY-MM-DD
- **Description**: [What was done]
- **Result**: [Outcome or learnings]

## Discovered During Work

### Future Enhancement: [Name]
- **Date Discovered**: YYYY-MM-DD
- **Description**: [What could be improved]
- **Priority**: High/Medium/Low
"""
```

### Integration Points
```yaml
DOCUMENTATION:
  - integrates with: CLAUDE.md
  - referenced by: AI assistants at conversation start
  
WORKFLOW:
  - PLANNING.md: Read at session start
  - TASK.md: Updated throughout work
  - Both files: Source of truth for project state
```

## Validation Loop

### Level 1: File Creation
```bash
# Verify files exist
ls -la PLANNING.md TASK.md

# Expected: Both files exist with content
```

### Level 2: Content Validation
```bash
# Check PLANNING.md has required sections
grep -E "^## (Project Overview|Architecture|Goals|Style|Constraints)" PLANNING.md

# Check TASK.md has tracking structure  
grep -E "^## (Active Tasks|Completed Tasks|Discovered During Work)" TASK.md

# Expected: All sections present
```

### Level 3: Integration Test
```python
# Verify CLAUDE.md references work
# Simulate AI assistant reading files

def test_planning_readable():
    """PLANNING.md provides clear project context"""
    with open('PLANNING.md', 'r') as f:
        content = f.read()
        assert "Architecture" in content
        assert "Goals" in content
        assert len(content) > 1000  # Substantial content

def test_task_tracking():
    """TASK.md supports task management"""
    with open('TASK.md', 'r') as f:
        content = f.read()
        assert "Date Added" in content
        assert "Status" in content
```

## Final validation Checklist
- [ ] PLANNING.md created with all required sections
- [ ] TASK.md created with tracking structure
- [ ] Both files follow existing markdown patterns
- [ ] CLAUDE.md file references are valid
- [ ] Content is comprehensive and useful
- [ ] Files are properly formatted
- [ ] Integration with workflow verified

---

## Anti-Patterns to Avoid
- ❌ Don't create minimal stub files - include real content
- ❌ Don't skip sections mentioned in CLAUDE.md
- ❌ Don't use inconsistent formatting
- ❌ Don't make files AI assistants can't parse easily
- ❌ Don't forget to include dates in TASK.md
- ❌ Don't create overly complex structures