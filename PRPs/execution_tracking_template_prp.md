name: "Execution Tracking Template PRP"
description: |

## Purpose
Create a comprehensive execution tracking template that enables detailed progress monitoring, time tracking, task breakdowns, validation checkpoints, and lessons learned capture during PRP execution.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build execution_tracking_template.md that provides a structured way to track PRP execution progress, capture time spent, monitor task completion, record validation results, and document lessons learned.

## Why
- Visibility into execution progress improves success rates
- Time tracking enables better estimates
- Task breakdowns prevent missing components
- Validation checkpoints catch issues early
- Lessons learned improve future executions

## What
Create tracking template with:
- Progress tracking tables
- Time tracking by phase
- Component task breakdowns
- Validation checkpoint records
- Issue tracking section
- Lessons learned capture
- Automated tracking tools

### Success Criteria
- [ ] Template covers all execution phases
- [ ] Time tracking is granular and useful
- [ ] Task breakdowns are comprehensive
- [ ] Validation checkpoints are clear
- [ ] Issue tracking supports debugging
- [ ] Lessons learned format is actionable
- [ ] Template is easy to update during execution

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: docs/guides/execute_prp.md
  why: Understand phases that need tracking
  
- file: docs/guides/execute_prp_quick_start.md
  why: Quick execution also needs tracking
  
- file: .claude/commands/execute-prp.md
  why: Integration with execution command

- file: CLAUDE.md
  why: Task tracking requirements from TASK.md

- file: docs/scripts/progress_dashboard.py
  why: Automated tracking integration
```

### Current Codebase tree
```bash
context-engineering-intro/
├── docs/
│   ├── guides/          # Execution guides exist
│   └── scripts/         # Dashboard exists
└── (no tracking template exists)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── templates/
│   │   ├── execution_tracking_template.md   # Main tracking template
│   │   ├── tracking_example_filled.md       # Example filled template
│   │   └── quick_tracking_template.md       # Simplified version
│   └── scripts/
│       └── tracking_automation.py           # Auto-update tracking
```

### Known Gotchas
```python
# CRITICAL: Template must be fillable during execution
# Not a post-mortem document

# CRITICAL: Time tracking should be easy
# Use simple formats like "10:15-10:45 (30m)"

# CRITICAL: Must work with version control
# Changes should be meaningful in git diff

# CRITICAL: Balance detail with usability
# Too detailed = abandoned, too simple = useless
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create main tracking template:
CREATE docs/templates/execution_tracking_template.md:
  - STRUCTURE: By execution phases
  - INCLUDE: Time tracking tables
  - ADD: Task checklists
  - PROVIDE: Issue tracking

Task 2 - Create filled example:
CREATE docs/templates/tracking_example_filled.md:
  - DEMONSTRATE: Proper usage
  - SHOW: Real execution data
  - INCLUDE: Common patterns
  - HIGHLIGHT: Best practices

Task 3 - Create quick template:
CREATE docs/templates/quick_tracking_template.md:
  - SIMPLIFY: For rapid execution
  - FOCUS: Essential tracking only
  - MAINTAIN: Key metrics
  - ENABLE: 5-minute updates

Task 4 - Create automation script:
CREATE docs/scripts/tracking_automation.py:
  - AUTOMATE: Time calculations
  - UPDATE: Progress percentages
  - GENERATE: Summary reports
  - INTEGRATE: With git commits
```

### Per task pseudocode

```markdown
# Task 1 - execution_tracking_template.md
# PRP Execution Tracking Template

**PRP Name**: [Your PRP Name]  
**Start Date**: YYYY-MM-DD  
**Target Completion**: YYYY-MM-DD  
**Actual Completion**: [To be filled]

## Executive Summary
<!-- Fill this after completion -->
- **Overall Success**: ⬜ Yes / ⬜ No / ⬜ Partial
- **Time Estimate Accuracy**: Estimated: __ hrs | Actual: __ hrs
- **Major Challenges**: [Brief summary]
- **Key Learnings**: [Top 3 insights]

## Phase 1: Load & Understand

### Time Tracking
| Activity | Start | End | Duration | Notes |
|----------|-------|-----|----------|-------|
| Initial PRP Read | 10:00 | 10:05 | 5m | [Any observations] |
| Reference Loading | 10:05 | 10:12 | 7m | [Files loaded] |
| Context Questions | 10:12 | 10:15 | 3m | [Questions raised] |

**Phase 1 Total**: __ minutes

### Checklist
- [ ] PRP read completely
- [ ] All references loaded
- [ ] Dependencies verified
- [ ] Mental model formed
- [ ] Questions answered

### Issues/Blockers
| Issue | Impact | Resolution | Time Lost |
|-------|---------|------------|-----------|
| [Description] | High/Med/Low | [How resolved] | __m |

## Phase 2: Research & Verify

### Time Tracking
| Activity | Start | End | Duration | Notes |
|----------|-------|-----|----------|-------|
| Pattern Search | | | | |
| Dependency Check | | | | |
| Documentation Review | | | | |

**Phase 2 Total**: __ minutes

### Discoveries
- **Similar Patterns Found**: 
  - `[file:line]` - [Pattern description]
  - `[file:line]` - [Pattern description]
  
- **Missing Information**:
  - [What was missing from PRP]
  - [How it was found]

### Research Commands Used
```bash
# Paste actual commands that were helpful
rg "pattern" --type py
find . -name "*.py" | xargs grep -l "concept"
```

## Phase 3: Plan & Structure

### Time Tracking
| Activity | Start | End | Duration | Notes |
|----------|-------|-----|----------|-------|
| Task Breakdown | | | | |
| Risk Assessment | | | | |
| Structure Design | | | | |

**Phase 3 Total**: __ minutes

### Task Breakdown
| Task # | Description | Est. Time | Actual Time | Status |
|--------|-------------|-----------|-------------|---------|
| 1 | Create data models | 15m | __m | ⬜ Not Started / ⬜ In Progress / ⬜ Complete |
| 2 | Implement core logic | 30m | __m | ⬜ Not Started / ⬜ In Progress / ⬜ Complete |
| 3 | Add error handling | 10m | __m | ⬜ Not Started / ⬜ In Progress / ⬜ Complete |
| 4 | Write tests | 20m | __m | ⬜ Not Started / ⬜ In Progress / ⬜ Complete |
| 5 | Integration | 15m | __m | ⬜ Not Started / ⬜ In Progress / ⬜ Complete |

### Risk Register
| Risk | Probability | Impact | Mitigation | Occurred? |
|------|------------|---------|------------|-----------|
| [Description] | H/M/L | H/M/L | [Strategy] | Y/N |

## Phase 4: Implement with Validation

### Time Tracking
| Component | Start | End | Duration | Validation | Issues |
|-----------|-------|-----|----------|------------|--------|
| Data Models | | | | ✓/✗ | |
| Core Logic | | | | ✓/✗ | |
| Integration | | | | ✓/✗ | |
| Error Handling | | | | ✓/✗ | |
| Tests | | | | ✓/✗ | |

**Phase 4 Total**: __ minutes

### Validation Results
```bash
# Quick validation outputs
./scripts/validate.sh --quick
[Paste results]

# Full validation outputs
./scripts/validate.sh --full
[Paste results]
```

### Code Metrics
- **Files Created**: __
- **Files Modified**: __
- **Lines of Code**: __
- **Test Coverage**: __%

## Phase 5: Finalize & Verify

### Time Tracking
| Activity | Start | End | Duration | Notes |
|----------|-------|-----|----------|-------|
| Final Validation | | | | |
| Documentation | | | | |
| Cleanup | | | | |

**Phase 5 Total**: __ minutes

### Success Criteria Verification
<!-- Check each item from PRP -->
- [ ] [Criterion 1 from PRP]
- [ ] [Criterion 2 from PRP]
- [ ] [Criterion 3 from PRP]

### Final Validation
```bash
# All tests passing
pytest -v
[Result summary]

# Linting clean
ruff check .
[Result summary]

# Manual testing
[Commands run and results]
```

## Lessons Learned

### What Went Well
1. **[Success 1]**: [Why it worked]
2. **[Success 2]**: [Why it worked]
3. **[Success 3]**: [Why it worked]

### What Could Be Improved
1. **[Challenge 1]**: [Suggested improvement]
2. **[Challenge 2]**: [Suggested improvement]
3. **[Challenge 3]**: [Suggested improvement]

### Time Analysis
- **Most Time Consuming**: [Phase/Task] - [Why]
- **Quickest Win**: [Phase/Task] - [Why]
- **Unexpected Delays**: [What caused delays]

### Recommendations for Similar PRPs
1. [Specific advice]
2. [Specific advice]
3. [Specific advice]

## Summary Statistics

### Time Summary
| Phase | Estimated | Actual | Variance |
|-------|-----------|---------|----------|
| Phase 1 | 15m | __m | __% |
| Phase 2 | 20m | __m | __% |
| Phase 3 | 15m | __m | __% |
| Phase 4 | 60m | __m | __% |
| Phase 5 | 15m | __m | __% |
| **Total** | **125m** | **__m** | **__%** |

### Outcome Metrics
- **Success Rate**: __% (criteria met / total criteria)
- **First-Time Pass**: ⬜ Yes / ⬜ No
- **Rework Required**: __ hours
- **Confidence Score**: __/10

---
**Template Version**: 1.0  
**Last Updated**: [Date]

# Task 2 - tracking_example_filled.md
# PRP Execution Tracking Template (Example)

**PRP Name**: Weather CLI Tool  
**Start Date**: 2024-01-15  
**Target Completion**: 2024-01-15  
**Actual Completion**: 2024-01-15

## Executive Summary
- **Overall Success**: ✓ Yes
- **Time Estimate Accuracy**: Estimated: 2 hrs | Actual: 1.75 hrs
- **Major Challenges**: API rate limit handling required retry logic
- **Key Learnings**: 
  1. Always check for existing retry decorators
  2. Cache implementations save significant time
  3. CLI examples in repo were perfect patterns

[Continue with filled example...]

# Task 3 - quick_tracking_template.md
# Quick Execution Tracking

**PRP**: ________________  
**Date**: ________________

## Time Log
```
Start: ___:___
End:   ___:___
Total: ___ min
```

## Progress
- [ ] Context Loaded (___m)
- [ ] Patterns Found (___m)
- [ ] Scaffolded (___m)
- [ ] Core Built (___m)
- [ ] Tests Pass (___m)
- [ ] Validated (___m)

## Issues
1. _________________________ [Resolved: Y/N]
2. _________________________ [Resolved: Y/N]

## Key Learning
_________________________________

# Task 4 - tracking_automation.py
#!/usr/bin/env python3
"""Automate execution tracking updates."""

import re
from datetime import datetime, timedelta
from pathlib import Path
import git
import json

class TrackingAutomation:
    def __init__(self, template_path="execution_tracking.md"):
        self.template_path = Path(template_path)
        self.repo = git.Repo(search_parent_directories=True)
        
    def update_time_entry(self, phase, activity, start, end):
        """Update a time entry in the tracking document."""
        # Calculate duration
        start_time = datetime.strptime(start, "%H:%M")
        end_time = datetime.strptime(end, "%H:%M")
        duration = end_time - start_time
        minutes = int(duration.total_seconds() / 60)
        
        # Read current content
        content = self.template_path.read_text()
        
        # Update the specific row
        # Implementation here...
        
    def calculate_phase_totals(self):
        """Calculate and update phase totals."""
        content = self.template_path.read_text()
        
        # Parse time entries
        # Sum by phase
        # Update totals
        
    def generate_summary_report(self):
        """Generate a summary report."""
        # Parse the tracking document
        # Calculate statistics
        # Generate report
        
    def commit_progress(self, message="Update execution tracking"):
        """Commit tracking updates to git."""
        self.repo.index.add([str(self.template_path)])
        self.repo.index.commit(f"📊 {message}")

# CLI interface
if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser()
    # Add arguments
    # Implementation here...
```

### Integration Points
```yaml
TRACKING_INTEGRATION:
  - used during: All PRP executions
  - updated by: Developers and AI assistants
  - provides: Execution metrics
  
AUTOMATION:
  - git integration: Auto-commits progress
  - time calculation: Automated totals
  - report generation: Summary statistics
```

## Validation Loop

### Level 1: Template Completeness
```bash
# Check all phases present
grep -E "^## Phase [1-5]:" docs/templates/execution_tracking_template.md | wc -l
# Expected: 5

# Verify tables exist
grep -c "| Activity |" docs/templates/execution_tracking_template.md
# Expected: 5+
```

### Level 2: Usability Testing
```python
def test_template_fillable():
    '''Template has clear placeholders.'''
    with open('docs/templates/execution_tracking_template.md') as f:
        content = f.read()
    
    # Check for placeholders
    assert '[' in content  # Brackets for filling
    assert '__' in content  # Underscores for times
    assert '⬜' in content  # Checkboxes

def test_automation_works():
    '''Automation script functions.'''
    from docs.scripts.tracking_automation import TrackingAutomation
    
    tracker = TrackingAutomation("test_tracking.md")
    # Test basic functionality
```

## Final validation Checklist
- [ ] Main tracking template comprehensive
- [ ] Time tracking tables easy to use
- [ ] Task breakdowns cover all components
- [ ] Validation checkpoints included
- [ ] Issue tracking supports debugging
- [ ] Lessons learned format actionable
- [ ] Example shows proper usage
- [ ] Quick template for rapid tracking
- [ ] Automation script functional

---

## Anti-Patterns to Avoid
- ❌ Don't create overly complex templates
- ❌ Don't forget time tracking
- ❌ Don't skip lessons learned
- ❌ Don't make updates difficult
- ❌ Don't ignore automation opportunities
- ❌ Don't create templates that won't be used