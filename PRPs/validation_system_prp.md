name: "Validation System PRP"
description: |

## Purpose
Implement a comprehensive validation system that ensures code quality and completeness during PRP execution, enabling AI assistants to self-correct and achieve working implementations.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Create a validation system with scripts and tools that AI assistants can run during PRP execution to ensure code quality, catch errors early, and enable self-correction through iterative validation loops.

## Why
- Validation loops are critical for one-pass implementation success
- AI assistants need executable feedback to fix their own mistakes
- Early error detection prevents accumulation of issues
- Standardized validation ensures consistent quality
- Enables confidence scoring for implementations

## What
Build a validation framework that includes:
- Executable validation script (validate.sh)
- Python validation runner with phases
- Integration with common tools (ruff, mypy, pytest)
- Quick mode for development, full mode for completion
- Clear error reporting with actionable feedback
- Support for custom validation rules

### Success Criteria
- [ ] validate.sh script exists and is executable
- [ ] Python validation runner with phase support
- [ ] Integration with linting, type checking, and testing
- [ ] Color-coded output for easy reading
- [ ] Quick and full validation modes
- [ ] Validation templates for PRPs
- [ ] Documentation for adding custom validations

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: .claude/commands/execute-prp.md
  why: References validation throughout execution process
  
- file: PRPs/templates/prp_base.md
  why: Shows three-level validation structure expected
  
- file: CLAUDE.md
  why: Mentions linting and testing requirements

- file: README.md
  why: Discusses validation gates and self-correction

- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Shows validation steps in practice
```

### Current Codebase tree
```bash
context-engineering-intro/
├── .claude/
├── PRPs/
├── examples/
└── (no validation scripts exist yet)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── scripts/
│   ├── validate.sh          # Main validation script (executable)
│   ├── validate.py          # Python validation runner
│   └── validation_config.py # Validation configuration
├── .validation/
│   ├── __init__.py
│   ├── phases.py           # Validation phase definitions
│   ├── reporters.py        # Output formatting and reporting
│   └── custom_checks.py    # Project-specific validations
└── docs/
    └── validation_guide.md  # How to use and extend validation
```

### Known Gotchas
```python
# CRITICAL: validate.sh must work on multiple platforms
# Use /usr/bin/env bash shebang, handle Windows via Git Bash

# CRITICAL: Must gracefully handle missing tools
# Check for ruff, mypy, pytest before running

# CRITICAL: Exit codes must be meaningful
# 0 = success, 1 = validation failed, 2 = setup error

# CRITICAL: Output must be parseable by AI
# Clear error messages with file:line references
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create validation script:
CREATE scripts/validate.sh:
  - IMPLEMENT: Platform-agnostic bash script
  - SUPPORT: Quick and full modes
  - CHECK: Tool availability
  - PROVIDE: Colored output

Task 2 - Create Python validation runner:
CREATE scripts/validate.py:
  - IMPLEMENT: Phase-based validation
  - INTEGRATE: Multiple tools
  - REPORT: Clear, actionable errors
  - SUPPORT: Configuration

Task 3 - Create validation phases:
CREATE .validation/phases.py:
  - DEFINE: Syntax, Style, Tests phases
  - IMPLEMENT: Phase execution logic
  - HANDLE: Phase dependencies
  - TRACK: Phase results

Task 4 - Create reporters:
CREATE .validation/reporters.py:
  - IMPLEMENT: Console output formatting
  - ADD: Color coding for status
  - FORMAT: Error messages
  - SUMMARIZE: Results

Task 5 - Create custom checks:
CREATE .validation/custom_checks.py:
  - IMPLEMENT: Project-specific validations
  - CHECK: File size limits (per CLAUDE.md)
  - VERIFY: Documentation standards
  - VALIDATE: Import patterns

Task 6 - Create validation guide:
CREATE docs/validation_guide.md:
  - DOCUMENT: Usage instructions
  - EXPLAIN: Phase system
  - SHOW: Extension examples
  - INCLUDE: Troubleshooting
```

### Per task pseudocode

```bash
# Task 1 - validate.sh
#!/usr/bin/env bash
set -euo pipefail

# Color definitions
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Default mode
MODE="quick"
VERBOSE=false

# Parse arguments
while [[ $# -gt 0 ]]; do
  case $1 in
    --full)
      MODE="full"
      shift
      ;;
    --verbose|-v)
      VERBOSE=true
      shift
      ;;
    --help|-h)
      echo "Usage: $0 [--full] [--verbose]"
      echo "  --full    Run full validation (default: quick)"
      echo "  --verbose Show detailed output"
      exit 0
      ;;
    *)
      echo "Unknown option: $1"
      exit 1
      ;;
  esac
done

echo -e "${BLUE}🔍 Running validation in $MODE mode...${NC}"

# Check for required tools
check_tool() {
  if ! command -v "$1" &> /dev/null; then
    echo -e "${RED}❌ $1 not found. Please install it.${NC}"
    return 1
  fi
  return 0
}

# Phase 1: Syntax Check
echo -e "\n${BLUE}Phase 1: Syntax Check${NC}"
if check_tool "python"; then
  python -m py_compile **/*.py 2>&1 | grep -v "__pycache__" || true
  echo -e "${GREEN}✓ Python syntax valid${NC}"
fi

# Phase 2: Style Check
echo -e "\n${BLUE}Phase 2: Style Check${NC}"
if check_tool "ruff"; then
  if ruff check . --fix; then
    echo -e "${GREEN}✓ Style check passed${NC}"
  else
    echo -e "${YELLOW}⚠ Style issues found (some auto-fixed)${NC}"
  fi
fi

# Phase 3: Type Check (full mode only)
if [[ "$MODE" == "full" ]]; then
  echo -e "\n${BLUE}Phase 3: Type Check${NC}"
  if check_tool "mypy"; then
    if mypy . --ignore-missing-imports; then
      echo -e "${GREEN}✓ Type check passed${NC}"
    else
      echo -e "${RED}❌ Type errors found${NC}"
      exit 1
    fi
  fi
fi

# Phase 4: Tests (full mode only)
if [[ "$MODE" == "full" ]]; then
  echo -e "\n${BLUE}Phase 4: Running Tests${NC}"
  if check_tool "pytest"; then
    if pytest -v; then
      echo -e "${GREEN}✓ All tests passed${NC}"
    else
      echo -e "${RED}❌ Test failures${NC}"
      exit 1
    fi
  fi
fi

echo -e "\n${GREEN}✅ Validation complete!${NC}"
```

```python
# Task 2 - validate.py
'''Comprehensive validation runner for PRP execution.'''

import asyncio
import subprocess
import sys
from pathlib import Path
from typing import List, Dict, Any, Optional
from dataclasses import dataclass
from enum import Enum
import logging

from .validation.phases import ValidationPhase, PhaseResult
from .validation.reporters import ConsoleReporter, ValidationReport

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class ValidationMode(Enum):
    QUICK = "quick"
    FULL = "full"
    CUSTOM = "custom"

@dataclass
class ValidationConfig:
    '''Configuration for validation run.'''
    mode: ValidationMode = ValidationMode.QUICK
    verbose: bool = False
    fix_issues: bool = True
    fail_fast: bool = False
    phases: Optional[List[str]] = None

class ValidationRunner:
    '''Main validation orchestrator.'''
    
    def __init__(self, config: ValidationConfig):
        self.config = config
        self.phases = self._load_phases()
        self.reporter = ConsoleReporter(verbose=config.verbose)
        
    def _load_phases(self) -> List[ValidationPhase]:
        '''Load validation phases based on mode.'''
        from .validation.phases import (
            SyntaxPhase, StylePhase, TypeCheckPhase,
            TestPhase, CustomPhase
        )
        
        if self.config.mode == ValidationMode.QUICK:
            return [SyntaxPhase(), StylePhase()]
        elif self.config.mode == ValidationMode.FULL:
            return [
                SyntaxPhase(),
                StylePhase(),
                TypeCheckPhase(),
                TestPhase(),
                CustomPhase()
            ]
        else:
            # Custom mode - load specific phases
            phase_map = {
                'syntax': SyntaxPhase,
                'style': StylePhase,
                'types': TypeCheckPhase,
                'tests': TestPhase,
                'custom': CustomPhase
            }
            return [
                phase_map[name]()
                for name in self.config.phases or []
            ]
    
    async def run(self) -> ValidationReport:
        '''Run all validation phases.'''
        report = ValidationReport()
        
        self.reporter.start(self.config.mode)
        
        for phase in self.phases:
            self.reporter.start_phase(phase)
            
            try:
                result = await phase.execute(
                    fix=self.config.fix_issues
                )
                report.add_result(phase, result)
                self.reporter.report_phase(phase, result)
                
                if not result.success and self.config.fail_fast:
                    break
                    
            except Exception as e:
                logger.error(f"Phase {phase.name} failed: {e}")
                result = PhaseResult(
                    success=False,
                    errors=[str(e)],
                    warnings=[]
                )
                report.add_result(phase, result)
                
                if self.config.fail_fast:
                    break
        
        self.reporter.finish(report)
        return report

async def main():
    '''CLI entry point.'''
    import argparse
    
    parser = argparse.ArgumentParser(
        description='Run validation for PRP execution'
    )
    parser.add_argument(
        '--mode',
        choices=['quick', 'full', 'custom'],
        default='quick',
        help='Validation mode'
    )
    parser.add_argument(
        '--verbose', '-v',
        action='store_true',
        help='Verbose output'
    )
    parser.add_argument(
        '--no-fix',
        action='store_true',
        help='Disable auto-fixing'
    )
    parser.add_argument(
        '--fail-fast',
        action='store_true',
        help='Stop on first failure'
    )
    parser.add_argument(
        '--phases',
        nargs='+',
        help='Specific phases for custom mode'
    )
    
    args = parser.parse_args()
    
    config = ValidationConfig(
        mode=ValidationMode(args.mode),
        verbose=args.verbose,
        fix_issues=not args.no_fix,
        fail_fast=args.fail_fast,
        phases=args.phases
    )
    
    runner = ValidationRunner(config)
    report = await runner.run()
    
    sys.exit(0 if report.all_passed else 1)

if __name__ == '__main__':
    asyncio.run(main())
```

```python
# Task 3 - phases.py
'''Validation phase definitions.'''

from abc import ABC, abstractmethod
from dataclasses import dataclass
from typing import List, Optional
import subprocess
import asyncio

@dataclass
class PhaseResult:
    '''Result of a validation phase.'''
    success: bool
    errors: List[str]
    warnings: List[str]
    fixed_count: int = 0

class ValidationPhase(ABC):
    '''Base validation phase.'''
    
    def __init__(self, name: str, description: str):
        self.name = name
        self.description = description
    
    @abstractmethod
    async def execute(self, fix: bool = True) -> PhaseResult:
        '''Execute the validation phase.'''
        pass
    
    async def _run_command(
        self,
        cmd: List[str],
        check: bool = True
    ) -> tuple[int, str, str]:
        '''Run a command and return results.'''
        proc = await asyncio.create_subprocess_exec(
            *cmd,
            stdout=asyncio.subprocess.PIPE,
            stderr=asyncio.subprocess.PIPE
        )
        
        stdout, stderr = await proc.communicate()
        
        return (
            proc.returncode,
            stdout.decode('utf-8'),
            stderr.decode('utf-8')
        )

class SyntaxPhase(ValidationPhase):
    '''Check Python syntax.'''
    
    def __init__(self):
        super().__init__(
            "Syntax Check",
            "Verify all Python files have valid syntax"
        )
    
    async def execute(self, fix: bool = True) -> PhaseResult:
        '''Run syntax validation.'''
        from pathlib import Path
        import ast
        
        errors = []
        python_files = Path('.').rglob('*.py')
        
        for file in python_files:
            if '__pycache__' in str(file):
                continue
                
            try:
                with open(file, 'r') as f:
                    ast.parse(f.read())
            except SyntaxError as e:
                errors.append(
                    f"{file}:{e.lineno}: {e.msg}"
                )
        
        return PhaseResult(
            success=len(errors) == 0,
            errors=errors,
            warnings=[]
        )

class StylePhase(ValidationPhase):
    '''Check code style with ruff.'''
    
    def __init__(self):
        super().__init__(
            "Style Check",
            "Verify code follows style guidelines"
        )
    
    async def execute(self, fix: bool = True) -> PhaseResult:
        '''Run style validation.'''
        cmd = ['ruff', 'check', '.']
        if fix:
            cmd.append('--fix')
        
        returncode, stdout, stderr = await self._run_command(
            cmd,
            check=False
        )
        
        errors = []
        warnings = []
        fixed_count = 0
        
        if returncode != 0:
            # Parse ruff output
            for line in stdout.split('\n'):
                if ': E' in line:  # Error
                    errors.append(line.strip())
                elif ': W' in line:  # Warning
                    warnings.append(line.strip())
                elif 'fixed' in line.lower():
                    # Extract fixed count
                    import re
                    match = re.search(r'(\d+) fixed', line)
                    if match:
                        fixed_count = int(match.group(1))
        
        return PhaseResult(
            success=len(errors) == 0,
            errors=errors,
            warnings=warnings,
            fixed_count=fixed_count
        )
```

### Integration Points
```yaml
VALIDATION_INTEGRATION:
  - called by: /execute-prp command
  - runs during: Each implementation phase
  - outputs to: Console with color coding
  
PRP_INTEGRATION:
  - referenced in: Validation Loop sections
  - provides: Executable commands
  - enables: Self-correction
```

## Validation Loop

### Level 1: Script Functionality
```bash
# Make script executable
chmod +x scripts/validate.sh

# Test help
./scripts/validate.sh --help

# Test quick mode
./scripts/validate.sh

# Expected: Colored output, proper execution
```

### Level 2: Python Runner
```bash
# Test Python validation runner
python -m scripts.validate --mode quick

# Test with verbose
python -m scripts.validate --mode full --verbose

# Expected: Detailed phase output
```

### Level 3: Integration Test
```python
# Test that validation catches real issues
def test_validation_catches_errors():
    '''Validation identifies code issues.'''
    
    # Create a file with issues
    bad_code = '''
def bad_function(x):
    y = x + 1
    return z  # NameError
    '''
    
    with open('test_bad.py', 'w') as f:
        f.write(bad_code)
    
    # Run validation
    result = subprocess.run(
        ['./scripts/validate.sh', '--full'],
        capture_output=True
    )
    
    assert result.returncode != 0
    assert 'error' in result.stdout.decode().lower()
    
    # Cleanup
    os.unlink('test_bad.py')

def test_validation_passes_good_code():
    '''Validation passes for good code.'''
    # Run on examples directory
    result = subprocess.run(
        ['./scripts/validate.sh', '--full'],
        cwd='examples',
        capture_output=True
    )
    
    assert result.returncode == 0
```

## Final validation Checklist
- [ ] validate.sh script is executable and cross-platform
- [ ] Python validation runner with phase support works
- [ ] All validation phases implemented
- [ ] Color-coded output for readability
- [ ] Quick and full modes function correctly
- [ ] Integration with ruff, mypy, pytest
- [ ] Clear error messages with file:line format
- [ ] Documentation explains usage and extension
- [ ] Custom validation checks implemented

---

## Anti-Patterns to Avoid
- ❌ Don't create platform-specific scripts
- ❌ Don't ignore exit codes
- ❌ Don't produce unclear error messages
- ❌ Don't skip validation phases
- ❌ Don't hide errors in verbose output
- ❌ Don't make validation too slow for quick mode