name: "Validation Runner Implementation PRP"
description: |

## Purpose
Create the validation_runner.md guide that provides a systematic validation framework with customizable validation scripts, phase-based validation approaches, CI/CD integration examples, and common issue fixes.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build validation_runner.md that provides a comprehensive validation framework including systematic approaches, customizable scripts, phase-based validation, CI/CD integration patterns, and solutions for common validation issues.

## Why
- Systematic validation prevents accumulation of errors
- Customizable scripts adapt to project needs
- Phase-based approach ensures progressive quality
- CI/CD integration enables automation
- Common issue fixes reduce debugging time

## What
Create validation runner guide with:
- Systematic validation framework
- Customizable validation scripts
- Phase-based validation approach
- CI/CD integration examples
- Common issue fixes
- Performance optimization
- Custom validation rules

### Success Criteria
- [ ] Framework covers all validation types
- [ ] Scripts are customizable and extensible
- [ ] Phase approach is clear and logical
- [ ] CI/CD examples work with major platforms
- [ ] Common issues have clear solutions
- [ ] Performance considerations included
- [ ] Integration with existing tools smooth

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: PRPs/validation_system_prp.md
  why: Base validation system to build upon
  
- file: scripts/validate.sh
  why: Current validation script to enhance
  
- file: .validation/phases.py
  why: Phase definitions to document

- file: CLAUDE.md
  why: Validation requirements

- file: .claude/commands/execute-prp.md
  why: How validation integrates with execution
```

### Current Codebase tree
```bash
context-engineering-intro/
├── scripts/
│   ├── validate.sh          # Basic validation exists
│   └── validate.py          # Python runner exists
├── .validation/             # Validation modules exist
└── (no comprehensive guide)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── guides/
│   │   └── validation_runner.md        # Comprehensive validation guide
│   ├── reference/
│   │   ├── validation_rules.md         # All validation rules
│   │   └── validation_matrix.md        # What to validate when
│   └── examples/
│       ├── github_actions_validation.yml
│       ├── gitlab_ci_validation.yml
│       └── jenkins_validation.groovy
```

### Known Gotchas
```python
# CRITICAL: Validation must be fast in development
# Slow validation = skipped validation

# CRITICAL: Error messages must be actionable
# "Test failed" vs "Missing type hint on line 42"

# CRITICAL: Support partial validation
# Can validate single files or directories

# CRITICAL: Handle missing dependencies gracefully
# Not everyone has all tools installed
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create validation runner guide:
CREATE docs/guides/validation_runner.md:
  - EXPLAIN: Validation philosophy
  - PROVIDE: Framework overview
  - DETAIL: Each validation type
  - INCLUDE: Customization guide

Task 2 - Create validation rules reference:
CREATE docs/reference/validation_rules.md:
  - CATALOG: All validation rules
  - EXPLAIN: Why each matters
  - SHOW: How to fix violations
  - PROVIDE: Configuration options

Task 3 - Create validation matrix:
CREATE docs/reference/validation_matrix.md:
  - MAP: Validation to development stages
  - PRIORITIZE: Critical vs nice-to-have
  - TIME: Estimates for each check
  - GUIDE: What to skip when

Task 4 - Create CI/CD examples:
CREATE docs/examples/*_validation.yml:
  - IMPLEMENT: GitHub Actions workflow
  - IMPLEMENT: GitLab CI pipeline
  - IMPLEMENT: Jenkins pipeline
  - SHOW: Integration patterns
```

### Per task pseudocode

```markdown
# Task 1 - validation_runner.md
# Validation Runner: Systematic Quality Assurance

## Overview

The Validation Runner provides a comprehensive framework for ensuring code quality throughout PRP execution. It supports multiple validation levels, custom rules, and seamless CI/CD integration.

## Core Philosophy

### Progressive Validation
```
Level 1: Syntax     ← Every save (< 1 second)
Level 2: Style      ← Every commit (< 10 seconds)
Level 3: Types      ← Every push (< 30 seconds)
Level 4: Tests      ← Before merge (< 5 minutes)
Level 5: Integration ← Release prep (< 15 minutes)
```

### Fail Fast, Fix Early
- Catch errors at the earliest possible stage
- Provide immediate, actionable feedback
- Enable quick fix cycles

## The Validation Framework

### 1. Quick Validation (Development)

**Purpose**: Instant feedback during coding
**Frequency**: After each file save or component completion
**Duration**: < 5 seconds

```bash
# Quick validation command
./scripts/validate.sh --quick

# What it checks:
# - Python syntax
# - Import errors  
# - Basic style issues
# - File size limits
```

**Integration with Editors**:
```json
// VS Code settings.json
{
  "python.linting.enabled": true,
  "python.linting.ruffEnabled": true,
  "editor.formatOnSave": true,
  "files.watcherExclude": {
    "**/__pycache__/**": true
  }
}
```

### 2. Standard Validation (Pre-commit)

**Purpose**: Ensure commit quality
**Frequency**: Before each commit
**Duration**: < 30 seconds

```bash
# Standard validation
./scripts/validate.sh --standard

# What it checks:
# - All quick validation items
# - Type hints (mypy)
# - Docstring presence
# - Security issues
# - Complexity metrics
```

**Git Hook Integration**:
```bash
# .git/hooks/pre-commit
#!/bin/bash
./scripts/validate.sh --standard || {
    echo "Validation failed. Fix issues or use --no-verify to skip."
    exit 1
}
```

### 3. Full Validation (Pre-merge)

**Purpose**: Comprehensive quality check
**Frequency**: Before merging PRs
**Duration**: < 5 minutes

```bash
# Full validation
./scripts/validate.sh --full

# What it checks:
# - All standard validation items
# - Full test suite
# - Test coverage
# - Documentation completeness
# - Integration tests
# - Performance benchmarks
```

## Customizing Validation

### Adding Custom Rules

```python
# .validation/custom_rules.py
from validation.base import ValidationRule

class NoCurseWordsRule(ValidationRule):
    """Ensure professional language in code."""
    
    name = "no-curse-words"
    severity = "warning"
    
    def check(self, file_path: Path) -> list[Issue]:
        issues = []
        curse_words = ['damn', 'hell', 'crap']  # Add more
        
        with open(file_path) as f:
            for line_no, line in enumerate(f, 1):
                for word in curse_words:
                    if word in line.lower():
                        issues.append(Issue(
                            file=file_path,
                            line=line_no,
                            message=f"Unprofessional language: '{word}'"
                        ))
        
        return issues
```

### Configuration File

```yaml
# .validation/config.yml
validation:
  # Global settings
  fail_on_warnings: false
  parallel_jobs: 4
  
  # Tool-specific settings
  ruff:
    line_length: 100
    ignore:
      - E501  # Line too long (handled by formatter)
      - D100  # Missing module docstring
  
  mypy:
    ignore_missing_imports: true
    strict_optional: true
  
  pytest:
    min_coverage: 80
    markers:
      - slow: "marks tests as slow"
      - integration: "marks tests as integration"
  
  # Custom rules
  custom:
    - no-curse-words
    - max-file-size: 500
    - require-type-hints
```

## Phase-Based Validation

### Phase 1: Syntax & Structure
```python
class SyntaxValidation:
    """Ensures code is parseable."""
    
    def validate(self, files: list[Path]) -> ValidationResult:
        for file in files:
            try:
                with open(file) as f:
                    compile(f.read(), file, 'exec')
            except SyntaxError as e:
                return ValidationResult(
                    success=False,
                    error=f"{file}:{e.lineno}: {e.msg}"
                )
        return ValidationResult(success=True)
```

### Phase 2: Style & Formatting
```bash
# Using ruff for Python
ruff check . --fix

# Output parsing for CI
ruff check . --format=json > ruff-report.json
```

### Phase 3: Type Safety
```bash
# Progressive type checking
mypy --follow-imports=silent --incremental

# Strict mode for critical modules
mypy src/core --strict
```

### Phase 4: Test Execution
```bash
# Smart test selection
pytest --testmon  # Only run affected tests

# Parallel execution
pytest -n auto  # Use all CPU cores

# Coverage with thresholds
pytest --cov=src --cov-fail-under=80
```

### Phase 5: Integration Validation
```python
# Integration test pattern
@pytest.mark.integration
async def test_api_integration():
    """Test real API calls."""
    async with APIClient() as client:
        response = await client.get("/health")
        assert response.status == 200
```

## CI/CD Integration

### GitHub Actions
```yaml
# .github/workflows/validation.yml
name: Validation Pipeline

on: [push, pull_request]

jobs:
  quick-validation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Quick Validation
        run: |
          pip install -r requirements-dev.txt
          ./scripts/validate.sh --quick
        
  full-validation:
    needs: quick-validation
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
      
      - name: Full Validation
        run: |
          pip install -r requirements-dev.txt
          ./scripts/validate.sh --full
          
      - name: Upload Reports
        uses: actions/upload-artifact@v3
        with:
          name: validation-reports
          path: |
            coverage.xml
            ruff-report.json
            mypy-report.xml
```

### Performance Optimization

```python
# Parallel validation runner
import asyncio
from concurrent.futures import ProcessPoolExecutor

class ParallelValidator:
    def __init__(self, max_workers=None):
        self.executor = ProcessPoolExecutor(max_workers)
    
    async def validate_all(self, files: list[Path]):
        # Group files by type for efficient processing
        py_files = [f for f in files if f.suffix == '.py']
        md_files = [f for f in files if f.suffix == '.md']
        
        # Run validators in parallel
        tasks = [
            self.validate_python(py_files),
            self.validate_markdown(md_files),
        ]
        
        results = await asyncio.gather(*tasks)
        return self.merge_results(results)
```

## Common Issues and Fixes

### Issue: "Import Error During Validation"
```bash
# Fix: Ensure PYTHONPATH is set
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Or use relative imports
python -m pytest tests/
```

### Issue: "Validation Too Slow"
```bash
# Fix 1: Use incremental validation
mypy --incremental

# Fix 2: Parallelize
pytest -n auto

# Fix 3: Cache results
ruff check --cache-dir=.ruff_cache
```

### Issue: "Different Results Locally vs CI"
```bash
# Fix: Use exact versions
pip freeze > requirements-lock.txt
pip install -r requirements-lock.txt

# Ensure same Python version
python --version  # Should match CI
```

### Issue: "Too Many False Positives"
```python
# Fix: Configure tool-specific ignores
# pyproject.toml
[tool.ruff]
ignore = [
    "D100",  # Missing module docstring
    "D104",  # Missing public package docstring
]

[tool.mypy]
ignore_missing_imports = true

# For specific lines
code = "value"  # noqa: E501
```

## Validation Best Practices

1. **Start Simple**: Begin with syntax/style, add more over time
2. **Be Pragmatic**: 80% coverage is often better than 100%
3. **Fix Forward**: Don't validate old code unless modifying
4. **Cache Aggressively**: Validation should be fast
5. **Fail Gracefully**: Missing tools shouldn't block development

## Monitoring Validation Health

```python
# Track validation metrics
class ValidationMetrics:
    def __init__(self):
        self.metrics = {
            'syntax_errors': 0,
            'style_issues': 0,
            'type_errors': 0,
            'test_failures': 0,
            'validation_time': 0
        }
    
    def report(self):
        """Generate validation health report."""
        return {
            'health_score': self.calculate_health(),
            'trend': self.calculate_trend(),
            'bottlenecks': self.identify_bottlenecks()
        }
```

## Conclusion

Effective validation is about finding the right balance between thoroughness and speed. Use this framework as a starting point and adapt it to your project's needs.

Remember: The best validation is the one that actually gets run.

# Task 2 - validation_rules.md
# Validation Rules Reference

## Syntax Rules

### SYN001: Valid Python Syntax
**Severity**: Error  
**Tool**: Python AST  
**Fix**: Check error message for line number and syntax issue

### SYN002: Valid Import Statements
**Severity**: Error  
**Tool**: Import checker  
**Fix**: Ensure all imports exist and are properly formatted

[Continue with all rules...]
```

### Integration Points
```yaml
VALIDATION_RUNNER_INTEGRATION:
  - enhances: Basic validation system
  - provides: Comprehensive framework
  - enables: CI/CD automation
  
TOOLS:
  - integrates: ruff, mypy, pytest
  - supports: Custom validators
  - enables: Parallel execution
```

## Validation Loop

### Level 1: Guide Completeness
```bash
# Check all sections present
grep -E "^## (Overview|Core Philosophy|The Validation Framework|CI/CD Integration)" docs/guides/validation_runner.md

# Verify examples included
grep -c "```" docs/guides/validation_runner.md
# Expected: 20+ code blocks
```

### Level 2: Example Testing
```bash
# Test GitHub Actions example
yamllint docs/examples/github_actions_validation.yml

# Verify scripts referenced exist
grep "validate.sh" docs/guides/validation_runner.md
```

### Level 3: Integration Testing
```python
def test_custom_rule_example():
    '''Custom rule example is valid Python.'''
    import ast
    
    # Extract custom rule code from guide
    with open('docs/guides/validation_runner.md') as f:
        content = f.read()
    
    # Find the NoCurseWordsRule class
    # Verify it's valid Python
    # Test basic functionality

def test_ci_examples():
    '''CI/CD examples are valid.'''
    import yaml
    
    # Load and validate YAML
    for ci_file in Path('docs/examples').glob('*_validation.yml'):
        with open(ci_file) as f:
            yaml.safe_load(f)  # Should not error
```

## Final validation Checklist
- [ ] Validation framework comprehensive
- [ ] Phase-based approach clear
- [ ] Customization well documented
- [ ] CI/CD examples for major platforms
- [ ] Common issues have solutions
- [ ] Performance optimization included
- [ ] Integration with existing tools
- [ ] Best practices section helpful

---

## Anti-Patterns to Avoid
- ❌ Don't create overly strict validation
- ❌ Don't ignore performance impact
- ❌ Don't forget about incremental validation
- ❌ Don't make error messages vague
- ❌ Don't require all tools installed
- ❌ Don't validate without caching