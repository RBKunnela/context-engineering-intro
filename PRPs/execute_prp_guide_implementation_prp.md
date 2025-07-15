name: "Execute PRP Guide Implementation PRP"
description: |

## Purpose
Create the comprehensive execute_prp.md guide that provides a complete 5-phase methodology for transforming PRPs into working code through systematic execution.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build the main execution guide (execute_prp.md) that provides a complete methodology for executing PRPs, including the 5-phase process, research techniques, implementation patterns, and troubleshooting strategies.

## Why
- AI assistants need systematic execution methodology
- Reduces implementation errors through structured approach
- Ensures completeness with phase-based validation
- Provides troubleshooting for common issues
- Enables consistent high-quality implementations

## What
Create the execute_prp.md guide with:
- 5-phase execution methodology
- Detailed step-by-step process
- Research extension techniques
- Implementation patterns
- Troubleshooting guide
- Success metrics
- Integration with validation

### Success Criteria
- [ ] Complete 5-phase methodology documented
- [ ] Each phase has clear entry/exit criteria
- [ ] Research techniques explained with examples
- [ ] Implementation patterns provided
- [ ] Troubleshooting section covers common issues
- [ ] Integration with validation system clear

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: .claude/commands/execute-prp.md
  why: Current execution command to align with
  
- file: PRPs/templates/prp_base.md
  why: Understand PRP structure for execution
  
- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Example of complex PRP to reference

- file: CLAUDE.md
  why: Rules that execution must follow

- file: README.md
  why: Context on execution workflow
```

### Current Codebase tree
```bash
context-engineering-intro/
├── .claude/commands/execute-prp.md  # Simple command
├── PRPs/                           # Contains PRPs to execute
└── (no comprehensive guide exists)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── guides/
│   │   ├── execute_prp.md         # Comprehensive execution guide
│   │   └── execution_phases.md    # Detailed phase documentation
│   └── reference/
│       └── execution_checklist.md  # Quick reference checklist
```

### Known Gotchas
```python
# CRITICAL: Guide must be actionable, not theoretical
# Each step should have concrete actions

# CRITICAL: Must handle partial implementations
# Guide recovery from failed attempts

# CRITICAL: Research phase is crucial
# Many failures come from insufficient research

# CRITICAL: Validation must be continuous
# Not just at the end
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create main execution guide:
CREATE docs/guides/execute_prp.md:
  - DEFINE: 5-phase methodology
  - EXPLAIN: Each phase in detail
  - PROVIDE: Concrete examples
  - INCLUDE: Decision trees

Task 2 - Create phase documentation:
CREATE docs/guides/execution_phases.md:
  - DETAIL: Each phase's activities
  - PROVIDE: Entry/exit criteria
  - SHOW: Phase transitions
  - INCLUDE: Validation points

Task 3 - Create execution checklist:
CREATE docs/reference/execution_checklist.md:
  - SUMMARIZE: Key steps
  - FORMAT: As actionable checklist
  - INCLUDE: Common gotchas
  - PROVIDE: Quick reference
```

### Per task pseudocode

```markdown
# Task 1 - execute_prp.md Structure
# Execute PRP: Complete Implementation Guide

## Overview
This guide provides a systematic approach to executing PRPs (Product Requirements Prompts) to achieve working implementations in a single pass.

## The 5-Phase Execution Methodology

### Phase 1: Load & Understand (10-15 minutes)
**Goal**: Fully internalize the PRP and its context

#### Steps:
1. **Deep Read** the PRP
   - Read the entire PRP twice
   - Note all referenced files and documentation
   - Identify critical requirements
   
2. **Load Context**
   ```bash
   # Load all referenced files
   cat [referenced-file-1]
   cat [referenced-file-2]
   
   # Visit all documentation URLs
   curl [documentation-url] | less
   ```

3. **Create Mental Model**
   - Draw component relationships
   - Identify data flows
   - Note integration points

#### Exit Criteria:
- [ ] Can explain the feature in one paragraph
- [ ] All referenced files loaded
- [ ] Documentation reviewed
- [ ] Dependencies understood

### Phase 2: Research & Verify (15-20 minutes)
**Goal**: Extend context beyond what's in the PRP

#### Research Techniques:

1. **Codebase Exploration**
   ```bash
   # Find similar patterns
   rg "class.*Agent" --type py
   
   # Understand imports
   rg "^import|^from.*import" [target-directory]
   
   # Check existing tests
   find . -name "*test*.py" -path "*/[feature-area]/*"
   ```

2. **Pattern Identification**
   - Look for naming conventions
   - Identify error handling patterns
   - Find configuration patterns

3. **Dependency Verification**
   ```bash
   # Check if libraries exist
   pip list | grep [library-name]
   
   # Verify versions
   python -c "import [library]; print([library].__version__)"
   ```

#### Common Research Patterns:
- **For APIs**: Check rate limits, auth methods, response formats
- **For CLIs**: Examine argument parsing, output formatting
- **For Agents**: Review state management, tool integration

### Phase 3: Plan & Structure (10-15 minutes)
**Goal**: Create detailed implementation plan

#### Planning Framework:

1. **Task Breakdown**
   - Use TodoWrite tool
   - Order by dependencies
   - Include validation after each task

2. **File Structure Planning**
   ```
   feature/
   ├── __init__.py
   ├── core.py          # Main logic
   ├── models.py        # Data structures
   ├── utils.py         # Helper functions
   └── tests/
       └── test_core.py # Tests
   ```

3. **Risk Identification**
   - External dependencies
   - Performance bottlenecks
   - Error scenarios

#### ULTRATHINK Technique:
```
1. Start with the end goal
2. Work backwards to identify steps
3. For each step, ask "what could go wrong?"
4. Add validation for each risk
```

### Phase 4: Implement with Validation (30-45 minutes)
**Goal**: Build the feature with continuous validation

#### Implementation Pattern:

1. **Start Simple**
   ```python
   # First: Get basic structure working
   def feature():
       return "TODO"
   
   # Validate: Can import without errors
   python -c "from feature import feature"
   ```

2. **Build Incrementally**
   - Implement one component
   - Validate it works
   - Commit mentally (note progress)
   - Move to next component

3. **Validation Rhythm**
   ```bash
   # After each component:
   ./scripts/validate.sh --quick
   
   # After major milestones:
   ./scripts/validate.sh --full
   ```

#### Common Implementation Order:
1. Data models/structures
2. Core business logic
3. Integration points
4. Error handling
5. CLI/API interface
6. Tests
7. Documentation

### Phase 5: Finalize & Verify (10-15 minutes)
**Goal**: Ensure everything meets PRP requirements

#### Final Checklist:

1. **Requirement Verification**
   - Re-read PRP success criteria
   - Check each item explicitly
   - Run acceptance tests

2. **Quality Validation**
   ```bash
   # Full validation suite
   ./scripts/validate.sh --full
   
   # Manual testing
   python -m feature --help
   python -m feature [example-usage]
   ```

3. **Documentation Check**
   - Docstrings complete
   - README updated if needed
   - Examples provided

## Troubleshooting Common Issues

### Issue: "Can't find similar patterns"
**Solution**: Expand search
```bash
# Search more broadly
rg "class.*" --type py | grep -i [concept]

# Check imports for clues
rg "from.*import" | grep -i [feature-area]
```

### Issue: "Tests keep failing"
**Solution**: Incremental debugging
```bash
# Run specific test
pytest path/to/test.py::test_function -v

# Add debug output
pytest -s  # Shows print statements
```

### Issue: "Import errors"
**Solution**: Check Python path
```python
import sys
sys.path.append('.')
```

## Advanced Techniques

### 1. Research Extensions
When the PRP lacks detail:

```bash
# Find similar PRPs
find PRPs -name "*.md" | xargs grep -l [similar-feature]

# Check git history
git log --grep=[feature] --oneline

# Search issues
gh issue list --search [feature]
```

### 2. Pattern Matching
```python
# Extract patterns programmatically
import ast
import inspect

# Find all class definitions
with open('example.py') as f:
    tree = ast.parse(f.read())
    classes = [n for n in ast.walk(tree) 
               if isinstance(n, ast.ClassDef)]
```

### 3. Validation Automation
```python
# Create custom validation
def validate_implementation():
    checks = [
        ("Imports work", test_imports),
        ("Models valid", test_models),
        ("API responds", test_api),
    ]
    
    for name, check in checks:
        try:
            check()
            print(f"✓ {name}")
        except Exception as e:
            print(f"✗ {name}: {e}")
            return False
    return True
```

## Success Metrics

Track your execution performance:

1. **Time to Completion**
   - Target: Under 2 hours for medium PRPs
   - Measure each phase

2. **Validation Passes**
   - Target: First-time success
   - Track iterations needed

3. **Requirement Coverage**
   - Target: 100% of success criteria
   - Check systematically

## Integration with Tools

### TodoWrite Integration
```python
todos = [
    "Load and understand PRP",
    "Research existing patterns", 
    "Plan implementation structure",
    "Implement core functionality",
    "Add error handling",
    "Write tests",
    "Run final validation"
]
```

### Validation Integration
- Quick validation after each component
- Full validation before marking complete
- Custom validation for special requirements

## Key Takeaways

1. **Research is crucial** - Spend time understanding
2. **Validate continuously** - Don't accumulate errors
3. **Follow patterns** - Consistency matters
4. **Plan before coding** - ULTRATHINK saves time
5. **Use the tools** - TodoWrite and validation

Remember: The goal is working code in one pass. This methodology makes that achievable.

# Task 2 - execution_phases.md
# Detailed Execution Phases

## Phase 1: Load & Understand

### Objectives
- Complete context acquisition
- Mental model formation
- Requirement clarity

### Activities
1. **Initial Read**
   - Time: 5 minutes
   - Goal: Overall understanding
   - Output: High-level summary

2. **Deep Read** 
   - Time: 5 minutes
   - Goal: Detail comprehension
   - Output: Questions list

3. **Context Loading**
   - Time: 5 minutes
   - Goal: Reference material
   - Output: Loaded files

### Entry Criteria
- [ ] PRP file available
- [ ] Development environment ready
- [ ] Tools accessible

### Exit Criteria
- [ ] Can explain feature purpose
- [ ] All references loaded
- [ ] Dependencies identified
- [ ] Questions answered

### Common Pitfalls
- Skipping documentation links
- Not loading example files
- Missing implicit requirements

[Continue for all phases...]
```

### Integration Points
```yaml
GUIDE_INTEGRATION:
  - references: validation system
  - uses: TodoWrite for planning
  - follows: CLAUDE.md rules
  
WORKFLOW:
  - triggered by: /execute-prp command
  - guides: systematic implementation
  - ensures: quality and completeness
```

## Validation Loop

### Level 1: Guide Completeness
```bash
# Check all sections present
grep -E "^## (Overview|The 5-Phase|Troubleshooting|Advanced|Success Metrics)" docs/guides/execute_prp.md

# Verify examples included
grep -c "```" docs/guides/execute_prp.md
# Expected: 15+ code blocks
```

### Level 2: Actionability Test
```python
def test_guide_actionable():
    '''Ensure guide provides concrete actions.'''
    
    with open('docs/guides/execute_prp.md', 'r') as f:
        content = f.read()
    
    # Check for action verbs
    action_verbs = ['Create', 'Run', 'Check', 'Verify', 'Test']
    for verb in action_verbs:
        assert verb in content
    
    # Check for commands
    assert '```bash' in content
    assert 'pytest' in content
    assert 'validate.sh' in content

def test_phase_criteria():
    '''Each phase has entry/exit criteria.'''
    
    with open('docs/guides/execution_phases.md', 'r') as f:
        content = f.read()
    
    phases = ['Phase 1', 'Phase 2', 'Phase 3', 'Phase 4', 'Phase 5']
    for phase in phases:
        assert f'{phase}:' in content
        assert 'Entry Criteria' in content
        assert 'Exit Criteria' in content
```

## Final validation Checklist
- [ ] 5-phase methodology clearly explained
- [ ] Each phase has concrete steps
- [ ] Research techniques documented
- [ ] Troubleshooting covers common issues
- [ ] Advanced techniques included
- [ ] Success metrics defined
- [ ] Integration with tools explained
- [ ] Examples throughout guide

---

## Anti-Patterns to Avoid
- ❌ Don't create vague theoretical guides
- ❌ Don't skip concrete examples
- ❌ Don't forget troubleshooting section
- ❌ Don't make phases too rigid
- ❌ Don't ignore tool integration
- ❌ Don't forget success metrics