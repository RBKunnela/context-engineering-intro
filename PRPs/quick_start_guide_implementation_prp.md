name: "Quick Start Guide Implementation PRP"
description: |

## Purpose
Create the execute_prp_quick_start.md guide that provides a streamlined 6-step fast track from PRP to production, including daily workflow templates, speed tips, and progress tracking.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build a quick start guide that enables rapid PRP execution through a streamlined 6-step process, with emphasis on speed, daily workflows, automation tips, and progress visibility.

## Why
- Users need a fast-track option for simple PRPs
- Daily workflow optimization improves productivity
- Speed tips reduce implementation time significantly
- Progress dashboards provide visibility
- Success criteria checklists ensure completeness

## What
Create the quick start guide with:
- 6-step streamlined process
- Daily workflow templates
- Speed optimization tips
- Progress dashboard script
- Success criteria checklist
- Automation strategies
- Time-saving shortcuts

### Success Criteria
- [ ] 6-step process clearly defined
- [ ] Daily workflow templates created
- [ ] Speed tips cover common scenarios
- [ ] Progress dashboard script functional
- [ ] Success checklist comprehensive
- [ ] Time estimates provided
- [ ] Automation examples included

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: .claude/commands/execute-prp.md
  why: Base execution process to streamline
  
- file: docs/guides/execute_prp.md
  why: Full guide to create quick version of
  
- file: PRPs/templates/prp_base.md
  why: Understanding what needs execution

- file: CLAUDE.md
  why: Rules for quick execution

- file: README.md
  why: Quick start section reference
```

### Current Codebase tree
```bash
context-engineering-intro/
├── docs/guides/execute_prp.md     # Full guide exists
└── (no quick start guide exists)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── guides/
│   │   ├── execute_prp_quick_start.md  # Quick 6-step guide
│   │   └── daily_workflow_template.md  # Daily execution template
│   └── scripts/
│       ├── progress_dashboard.py        # Progress tracking script
│       └── quick_validate.sh           # Streamlined validation
```

### Known Gotchas
```python
# CRITICAL: Quick doesn't mean sloppy
# Still need validation at key points

# CRITICAL: Time estimates must be realistic
# Based on medium-complexity PRPs

# CRITICAL: Dashboard must work without setup
# Self-contained Python script

# CRITICAL: Daily workflow must be memorable
# Use mnemonics or patterns
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create quick start guide:
CREATE docs/guides/execute_prp_quick_start.md:
  - DEFINE: 6-step process
  - INCLUDE: Time estimates
  - PROVIDE: Speed tips
  - ADD: Success checklist

Task 2 - Create daily workflow:
CREATE docs/guides/daily_workflow_template.md:
  - STRUCTURE: Morning/afternoon/evening
  - INCLUDE: Checkpoint system
  - ADD: Energy management
  - PROVIDE: Templates

Task 3 - Create progress dashboard:
CREATE docs/scripts/progress_dashboard.py:
  - IMPLEMENT: Task tracking
  - SHOW: Completion percentage
  - TRACK: Time spent
  - DISPLAY: Visual progress

Task 4 - Create quick validation:
CREATE docs/scripts/quick_validate.sh:
  - STREAMLINE: Validation steps
  - FOCUS: Critical checks only
  - PROVIDE: Fast feedback
  - SUPPORT: Fix suggestions
```

### Per task pseudocode

```markdown
# Task 1 - execute_prp_quick_start.md
# Execute PRP Quick Start: 6 Steps to Success

## ⚡ From PRP to Production in 6 Steps

### Overview
This quick start guide gets you from PRP to working code in under 90 minutes for medium-complexity features.

### The 6-Step Fast Track

#### Step 1: Speed Read & Load (5 minutes)
```bash
# Quick load all context
cat PRPs/[your-prp].md | head -100  # Get the gist
grep -E "^- file:|^- url:" PRPs/[your-prp].md  # Extract references

# Rapid reference load
for file in $(grep "file:" PRPs/[your-prp].md | cut -d: -f2); do
    echo "=== $file ==="
    head -20 "$file"
done
```

**Speed Tip**: Use split terminal - PRP on left, implementation on right

#### Step 2: Pattern Match (5 minutes)
```bash
# Find similar implementations instantly
rg "class.*[Similar]" --type py -A 5
find examples -name "*.py" | xargs grep -l "[pattern]"

# Quick dependency check
pip list | grep -E "(library1|library2|library3)"
```

**Speed Tip**: Keep a patterns.md file with common searches

#### Step 3: Scaffold Fast (10 minutes)
```python
# Use this template for rapid setup
"""
mkdir -p feature/{core,models,tests}
cat > feature/__init__.py << 'EOF'
"""Feature implementation."""
from .core import main_function
__all__ = ['main_function']
EOF

cat > feature/core.py << 'EOF'
"""Core implementation."""

def main_function():
    """TODO: Implement."""
    raise NotImplementedError()
EOF

# Immediate validation
python -c "from feature import main_function"
```

**Speed Tip**: Use snippets/templates for common structures

#### Step 4: Core Sprint (45 minutes)
**The Power Hour Pattern**:
- 0-15 min: Data models
- 15-30 min: Core logic  
- 30-40 min: Integration
- 40-45 min: Quick validation

```bash
# Validation checkpoints
*/15 * * * * ./docs/scripts/quick_validate.sh
```

**Speed Tip**: Set a timer - timeboxing prevents overthinking

#### Step 5: Test & Fix (15 minutes)
```bash
# Rapid test creation
cat > test_quick.py << 'EOF'
def test_basic():
    from feature import main_function
    # Test it works at all
    assert main_function is not None

def test_example():
    from feature import main_function
    result = main_function("example")
    assert result  # Basic check
EOF

# Quick test run
pytest test_quick.py -v

# If fails, quick fix cycle:
# 1. Read error
# 2. Fix obvious issue
# 3. Re-run
# 4. Don't overthink
```

#### Step 6: Polish & Ship (10 minutes)
```bash
# Final checklist runner
./docs/scripts/quick_validate.sh --final

# Quick documentation
echo "# Feature Name

## Usage
\`\`\`python
from feature import main_function
result = main_function('input')
\`\`\`

## Implementation Notes
- Based on PRP: [name]
- Validated: $(date)
" > feature/README.md
```

### ⏱️ Time Management

**90-Minute Schedule**:
- 0:00-0:05 - Load context
- 0:05-0:10 - Pattern match  
- 0:10-0:20 - Scaffold
- 0:20-1:05 - Core sprint
- 1:05-1:20 - Test & fix
- 1:20-1:30 - Polish

**When You're Behind Schedule**:
- Skip: Enhanced error handling
- Skip: Comprehensive tests
- Skip: Performance optimization
- Keep: Core functionality
- Keep: Basic validation

### 🚀 Speed Tips Collection

#### Terminal Productivity
```bash
# Split panes for efficiency
tmux new-session \; \
  split-window -h \; \
  split-window -v

# Pane 1: PRP viewing
# Pane 2: Coding
# Pane 3: Testing
```

#### Validation Shortcuts
```bash
# Add to ~/.bashrc
alias qval='./docs/scripts/quick_validate.sh'
alias qtest='pytest -xvs'  # Stop on first failure
```

#### Code Generation
```python
# Snippet for rapid model creation
def generate_model(name, fields):
    return f"""
class {name}:
    def __init__(self, {', '.join(fields)}):
        {chr(10).join(f'self.{f} = {f}' for f in fields)}
"""
```

### 📊 Success Criteria Checklist

```markdown
## Quick Success Checklist

### Must Have (Before calling it done)
- [ ] Core feature works
- [ ] Basic test passes
- [ ] No import errors
- [ ] Handles main use case

### Should Have (If time permits)
- [ ] Error handling
- [ ] Multiple test cases
- [ ] Documentation
- [ ] Type hints

### Nice to Have (Future iteration)
- [ ] Performance optimization
- [ ] Comprehensive tests
- [ ] Integration tests
- [ ] Detailed logging
```

### 🔧 Common Speed Bumps

**Problem**: Can't find patterns
**Quick Fix**: 
```bash
# Broader search
find . -name "*.py" -exec grep -l "similar_concept" {} \;
```

**Problem**: Import errors
**Quick Fix**:
```python
import sys
sys.path.insert(0, '.')
```

**Problem**: Test failures
**Quick Fix**: Start with simpler test:
```python
def test_imports():
    import feature  # Just test it imports
```

### 🎯 Daily Workflow Integration

**Morning Sprint** (Best for complex PRPs):
1. Fresh mind = better understanding
2. Load context with coffee
3. Core implementation before meetings

**Afternoon Push** (Best for simple PRPs):
1. Post-lunch energy burst
2. Quick wins for momentum
3. Test and polish

**Evening Wrap** (Best for fixes):
1. Fix validation issues
2. Polish documentation
3. Prep for next day

### 📈 Progress Tracking

Use the dashboard for visibility:
```bash
python docs/scripts/progress_dashboard.py
```

Output:
```
PRP Execution Progress
====================
Step 1: Load Context     [✓] 100% (5 min)
Step 2: Pattern Match    [✓] 100% (4 min)  
Step 3: Scaffold        [✓] 100% (8 min)
Step 4: Core Sprint     [▓▓▓░] 75% (35 min)
Step 5: Test & Fix      [░░░░] 0%
Step 6: Polish          [░░░░] 0%

Overall: 66% Complete
Time: 52 min / 90 min
ETA: 38 minutes
```

Remember: Speed comes from clarity, not rushing. Trust the process!

# Task 2 - daily_workflow_template.md
# Daily PRP Execution Workflow

## Morning Routine (High Energy)

### 8:00-8:15 - Context Loading
```bash
# Morning setup script
#!/bin/bash
echo "☕ Good morning! Loading context..."
cat ~/current_prp.md | head -50
echo "📋 Yesterday's progress:"
cat ~/progress.log | tail -10
```

### 8:15-8:30 - Planning Sprint
- Review PRP success criteria
- Update todo list
- Set 3 main goals for today

### 8:30-10:30 - Core Development
**The 2-Hour Power Block**
- No meetings
- No distractions  
- Core implementation

**Pomodoro Pattern**:
- 25 min: Code
- 5 min: Quick validate
- 25 min: Code
- 5 min: Stretch & review
- Repeat

[Continue with afternoon and evening workflows...]

# Task 3 - progress_dashboard.py
#!/usr/bin/env python3
"""PRP Execution Progress Dashboard"""

import time
import json
from datetime import datetime, timedelta
from pathlib import Path
import sys

class ProgressDashboard:
    def __init__(self, prp_name="current"):
        self.prp_name = prp_name
        self.progress_file = Path(f".progress_{prp_name}.json")
        self.steps = [
            ("Load Context", 5),
            ("Pattern Match", 5),
            ("Scaffold", 10),
            ("Core Sprint", 45),
            ("Test & Fix", 15),
            ("Polish", 10)
        ]
        self.load_progress()
    
    def load_progress(self):
        """Load existing progress or initialize."""
        if self.progress_file.exists():
            with open(self.progress_file) as f:
                self.data = json.load(f)
        else:
            self.data = {
                "started": datetime.now().isoformat(),
                "steps": {name: {"complete": 0, "time_spent": 0} 
                         for name, _ in self.steps}
            }
    
    def update_step(self, step_name, percent_complete):
        """Update progress for a step."""
        if step_name in self.data["steps"]:
            self.data["steps"][step_name]["complete"] = percent_complete
            self.save_progress()
    
    def save_progress(self):
        """Save progress to file."""
        with open(self.progress_file, 'w') as f:
            json.dump(self.data, f, indent=2)
    
    def display(self):
        """Display the dashboard."""
        print("\n" + "="*50)
        print(f"PRP Execution Progress: {self.prp_name}")
        print("="*50 + "\n")
        
        total_expected = sum(minutes for _, minutes in self.steps)
        total_complete = 0
        
        for step_name, expected_minutes in self.steps:
            step_data = self.data["steps"][step_name]
            complete = step_data["complete"]
            
            # Progress bar
            bar_length = 20
            filled = int(bar_length * complete / 100)
            bar = "█" * filled + "░" * (bar_length - filled)
            
            # Status
            status = "✓" if complete == 100 else "◐" if complete > 0 else "○"
            
            print(f"{status} {step_name:<20} [{bar}] {complete:>3}% ({expected_minutes} min)")
            
            total_complete += (complete / 100) * expected_minutes
        
        # Overall progress
        overall_percent = int((total_complete / total_expected) * 100)
        overall_bar_length = 40
        overall_filled = int(overall_bar_length * overall_percent / 100)
        overall_bar = "▓" * overall_filled + "░" * (overall_bar_length - overall_filled)
        
        print("\n" + "-"*50)
        print(f"Overall Progress: [{overall_bar}] {overall_percent}%")
        print(f"Time Estimate: {int(total_complete)} / {total_expected} minutes")
        
        # ETA
        if overall_percent > 0 and overall_percent < 100:
            eta_minutes = int(((100 - overall_percent) / overall_percent) * total_complete)
            print(f"ETA: {eta_minutes} minutes remaining")
        
        print("="*50 + "\n")

def main():
    """CLI interface."""
    import argparse
    
    parser = argparse.ArgumentParser(description="PRP Progress Dashboard")
    parser.add_argument("--update", nargs=2, metavar=("STEP", "PERCENT"),
                       help="Update step progress")
    parser.add_argument("--name", default="current", help="PRP name")
    
    args = parser.parse_args()
    
    dashboard = ProgressDashboard(args.name)
    
    if args.update:
        step_name, percent = args.update
        dashboard.update_step(step_name, int(percent))
        print(f"Updated {step_name} to {percent}%")
    
    dashboard.display()

if __name__ == "__main__":
    main()
```

### Integration Points
```yaml
QUICK_START_INTEGRATION:
  - supplements: Full execution guide
  - uses: Validation system
  - enables: Rapid implementation
  
WORKFLOW:
  - morning: High-complexity tasks
  - afternoon: Quick wins
  - tracking: Progress dashboard
```

## Validation Loop

### Level 1: Guide Testing
```bash
# Verify 6 steps are clear
grep -E "^#### Step [1-6]:" docs/guides/execute_prp_quick_start.md | wc -l
# Expected: 6

# Check time estimates
grep -E "[0-9]+ minutes?" docs/guides/execute_prp_quick_start.md | wc -l
# Expected: 10+
```

### Level 2: Dashboard Functionality
```bash
# Test progress dashboard
python docs/scripts/progress_dashboard.py --name test

# Update progress
python docs/scripts/progress_dashboard.py --name test --update "Load Context" 100

# Verify display works
python docs/scripts/progress_dashboard.py --name test
```

### Level 3: Workflow Integration
```python
def test_quick_validation():
    '''Quick validation script works.'''
    result = subprocess.run(
        ['bash', 'docs/scripts/quick_validate.sh'],
        capture_output=True
    )
    assert result.returncode in [0, 1]  # 0=pass, 1=issues found

def test_daily_workflow():
    '''Daily workflow template is actionable.'''
    with open('docs/guides/daily_workflow_template.md') as f:
        content = f.read()
    
    # Check for time blocks
    assert '8:00' in content
    assert 'Pomodoro' in content
    assert 'Power Block' in content
```

## Final validation Checklist
- [ ] 6-step process clear and actionable
- [ ] Time estimates realistic
- [ ] Speed tips cover common scenarios
- [ ] Progress dashboard functional
- [ ] Daily workflow templates complete
- [ ] Quick validation script works
- [ ] Success checklist comprehensive
- [ ] Integration points documented

---

## Anti-Patterns to Avoid
- ❌ Don't sacrifice quality for speed
- ❌ Don't skip validation checkpoints
- ❌ Don't create unrealistic time estimates
- ❌ Don't forget error handling entirely
- ❌ Don't make the guide too complex
- ❌ Don't ignore progress tracking