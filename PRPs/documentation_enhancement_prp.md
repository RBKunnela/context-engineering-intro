name: "Documentation Enhancement PRP"
description: |

## Purpose
Enhance and extend the Context Engineering documentation to provide comprehensive guidance, best practices, troubleshooting, and advanced usage patterns for users and AI assistants.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Create comprehensive documentation that covers all aspects of Context Engineering, from beginner tutorials to advanced patterns, troubleshooting guides, and integration strategies.

## Why
- Documentation is critical for adoption and correct usage
- Users need clear guidance on best practices
- AI assistants need detailed context for implementations
- Common issues should have documented solutions
- Advanced patterns enable power users

## What
Create extensive documentation including:
- Getting Started tutorial
- Best Practices guide
- Troubleshooting guide
- Advanced Patterns documentation
- Integration guides (IDEs, CI/CD)
- FAQ section
- Video script for tutorial
- Workflow diagrams

### Success Criteria
- [ ] All major use cases documented
- [ ] Clear progression from beginner to advanced
- [ ] Troubleshooting covers common issues
- [ ] Best practices based on real usage
- [ ] Visual aids (diagrams) included
- [ ] Documentation is searchable and well-organized

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: README.md
  why: Current documentation to enhance and extend
  
- file: .claude/commands/generate-prp.md
  why: Document the generation process in detail
  
- file: .claude/commands/execute-prp.md
  why: Document the execution process in detail

- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Use as example for documentation

- file: CLAUDE.md
  why: Document how to customize project rules

- file: INITIAL_EXAMPLE.md
  why: Show examples of good initial files
```

### Current Codebase tree
```bash
context-engineering-intro/
├── README.md          # Main documentation (needs enhancement)
├── docs/             # Will be created
└── ...
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── README.md                    # Enhanced main documentation
├── docs/
│   ├── getting-started.md      # Step-by-step tutorial
│   ├── best-practices.md       # Context engineering best practices
│   ├── troubleshooting.md      # Common issues and solutions
│   ├── advanced-patterns.md    # Complex scenarios and patterns
│   ├── integration/
│   │   ├── vscode.md          # VS Code integration
│   │   ├── jetbrains.md       # JetBrains IDE integration
│   │   ├── cicd.md            # CI/CD integration
│   │   └── github-actions.md  # GitHub Actions workflows
│   ├── faq.md                 # Frequently asked questions
│   ├── glossary.md            # Terms and definitions
│   ├── examples/
│   │   ├── web-scraper-prp.md    # Example PRP walkthrough
│   │   ├── api-client-prp.md     # API integration example
│   │   └── data-pipeline-prp.md  # Complex example
│   └── diagrams/
│       ├── workflow.mermaid       # PRP workflow diagram
│       ├── architecture.mermaid   # System architecture
│       └── validation-loop.mermaid # Validation process
└── CONTRIBUTING.md              # How to contribute
```

### Known Gotchas
```python
# CRITICAL: Documentation must be accessible to beginners
# Avoid jargon without explanation

# CRITICAL: Examples must be complete and tested
# Don't show partial code snippets without context

# CRITICAL: Keep documentation in sync with code
# Reference actual files and commands

# CRITICAL: Use consistent formatting
# Follow existing markdown patterns
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Enhance README.md:
EDIT README.md:
  - ADD: Quick wins section
  - EXPAND: Context vs Prompt engineering
  - ADD: Common mistakes section
  - IMPROVE: Examples section

Task 2 - Create Getting Started guide:
CREATE docs/getting-started.md:
  - WRITE: Complete tutorial from zero
  - INCLUDE: Screenshots/examples
  - COVER: First PRP creation
  - SHOW: Success criteria

Task 3 - Create Best Practices:
CREATE docs/best-practices.md:
  - DOCUMENT: INITIAL.md patterns
  - EXPLAIN: Example selection
  - SHOW: Documentation gathering
  - INCLUDE: Anti-patterns

Task 4 - Create Troubleshooting:
CREATE docs/troubleshooting.md:
  - LIST: Common errors
  - PROVIDE: Solutions
  - INCLUDE: Debug strategies
  - ADD: FAQ integration

Task 5 - Create Advanced Patterns:
CREATE docs/advanced-patterns.md:
  - DOCUMENT: Multi-file PRPs
  - SHOW: Complex integrations
  - EXPLAIN: Custom validations
  - INCLUDE: Performance tips

Task 6 - Create Integration guides:
CREATE docs/integration/*:
  - DOCUMENT: IDE setups
  - PROVIDE: CI/CD templates
  - SHOW: Automation patterns
  - INCLUDE: Tool configurations

Task 7 - Create Visual aids:
CREATE docs/diagrams/*:
  - DESIGN: Workflow diagrams
  - ILLUSTRATE: Architecture
  - SHOW: Process flows
  - USE: Mermaid format

Task 8 - Create Examples:
CREATE docs/examples/*:
  - WALKTHROUGH: Complete PRPs
  - SHOW: Real-world scenarios
  - EXPLAIN: Decision making
  - INCLUDE: Results
```

### Per task pseudocode

```markdown
# Task 1 - Enhanced README.md additions
## 🎯 Quick Wins with Context Engineering

### Your First PRP in 5 Minutes
1. **Clone and Setup** (30 seconds)
   ```bash
   git clone <repo>
   cd context-engineering-intro
   ```

2. **Create Your Feature Request** (2 minutes)
   Edit `INITIAL.md`:
   ```markdown
   FEATURE: Build a CLI tool that fetches weather data
   
   EXAMPLES: 
   - examples/cli/basic_cli.py - Use this CLI structure
   
   DOCUMENTATION:
   - https://openweathermap.org/api - Weather API docs
   
   OTHER CONSIDERATIONS:
   - Need API key handling
   - Should cache results for 5 minutes
   ```

3. **Generate PRP** (1 minute)
   ```bash
   /generate-prp INITIAL.md
   ```

4. **Execute** (90 seconds)
   ```bash
   /execute-prp PRPs/weather-cli.md
   ```

### ⚡ Why This Works
- **Complete Context**: The AI has everything it needs
- **Validation Loops**: Errors get fixed automatically
- **Pattern Following**: Your examples ensure consistency

# Task 2 - getting-started.md
# Getting Started with Context Engineering

Welcome! This guide will walk you through creating your first successful Context Engineering implementation in under 10 minutes.

## Prerequisites
- Claude Code installed
- Basic Python knowledge
- A feature you want to build

## Step 1: Understanding the System

Context Engineering has three main components:

1. **INITIAL.md** - Your feature request
2. **PRP** - The implementation blueprint
3. **Execution** - Automated implementation with validation

Think of it like this:
- INITIAL.md is your "what I want"
- PRP is the "detailed plan"
- Execution is the "make it happen"

## Step 2: Your First Feature Request

Let's build a real feature - a password strength checker CLI tool.

### 2.1 Create Your INITIAL.md

```markdown
FEATURE: 
Build a CLI tool that checks password strength with the following features:
- Check password against common patterns
- Provide strength score (0-100)
- Give specific improvement suggestions
- Support batch checking from file
- Output results in JSON or human-readable format

EXAMPLES:
- examples/cli/basic_cli.py - Follow this CLI structure
- examples/testing/test_patterns.py - Use these test patterns

DOCUMENTATION:
- https://pypi.org/project/password-strength/ - Can use this library
- https://github.com/dwyl/english-words - Common words list

OTHER CONSIDERATIONS:
- Should work offline (no API calls)
- Performance matters for batch checking
- Clear, actionable feedback for users
- Should handle Unicode passwords
```

### 2.2 Key Points
- **Be Specific**: "password strength checker" vs "security tool"
- **Reference Examples**: Point to patterns to follow
- **Include Docs**: External resources save research time
- **State Constraints**: Offline, performance, etc.

[Continue with full tutorial...]

# Task 3 - best-practices.md
# Context Engineering Best Practices

## 1. Writing Effective INITIAL.md Files

### 🎯 The Golden Rule
**Specificity beats brevity.** The more context you provide, the better the implementation.

### Structure for Success

#### FEATURE Section
```markdown
❌ Bad:
FEATURE: Build a web scraper

✅ Good:
FEATURE: Build an async web scraper that:
- Extracts product data (name, price, description, images)
- Handles pagination automatically
- Respects robots.txt and rate limits
- Saves data to PostgreSQL database
- Provides progress updates via websocket
```

#### EXAMPLES Section
```markdown
❌ Bad:
EXAMPLES: Use the examples folder

✅ Good:
EXAMPLES:
- examples/integrations/api_client.py - Use this retry pattern
- examples/agents/simple_agent.py - Follow this async structure
- examples/testing/fixtures.py - Use these test fixtures
```

### 2. Maximizing PRP Quality

#### Research Depth
The `/generate-prp` command researches your codebase. Help it by:

1. **Organizing Examples**: Group related patterns
2. **Clear Naming**: `auth_oauth_flow.py` not `auth2.py`
3. **Documentation**: Include README in examples/

#### Documentation Gathering
```yaml
GOOD Documentation Reference:
- url: https://docs.service.com/api/v2/auth
  why: Need OAuth2 flow specifically
  
- file: internal_docs/api_patterns.md
  why: Company standard for API integration
```

[Continue with all best practices...]

# Task 4 - troubleshooting.md  
# Troubleshooting Guide

## Common Issues and Solutions

### 1. PRP Generation Failures

#### Issue: "Could not find relevant patterns"
**Symptoms**: 
- PRP generation produces minimal context
- Confidence score below 7

**Solution**:
1. Add more examples to `examples/` folder
2. Make INITIAL.md more specific
3. Include direct file references

```markdown
# Add to INITIAL.md
EXAMPLES:
- examples/similar_feature.py - This is almost exactly what I want
```

#### Issue: "Documentation fetch failed"
**Symptoms**:
- Web fetch errors during generation
- Missing API documentation

**Solution**:
1. Save documentation locally
```bash
mkdir -p docs/external
wget https://api.example.com/docs -O docs/external/api.html
```

2. Reference local file
```markdown
DOCUMENTATION:
- file: docs/external/api.html
  why: API reference for integration
```

### 2. Execution Failures

#### Issue: "Validation keeps failing"
**Symptoms**:
- Same errors after multiple attempts
- AI can't fix linting issues

**Solution**:
1. Run validation manually
```bash
./scripts/validate.sh --verbose
```

2. Check for environment issues
```bash
# Ensure tools are installed
pip install ruff mypy pytest
```

3. Simplify the PRP
- Break into smaller tasks
- Remove complex validations temporarily

[Continue with more troubleshooting...]
```

### Integration Points
```yaml
DOCUMENTATION_INTEGRATION:
  - enhances: README.md
  - supports: New users and AI assistants
  - enables: Self-service troubleshooting
  
NAVIGATION:
  - docs/: Organized by topic
  - Cross-references: Link between related docs
  - Search-friendly: Clear headings and keywords
```

## Validation Loop

### Level 1: Documentation Structure
```bash
# Verify all planned files exist
find docs -name "*.md" | wc -l
# Expected: 15+ documentation files

# Check for broken links
grep -r "](/" docs/ | grep -v "http"
# Should reference valid paths
```

### Level 2: Content Quality
```python
def test_documentation_completeness():
    '''Ensure documentation covers key topics.'''
    
    required_sections = {
        'docs/getting-started.md': ['Prerequisites', 'Step 1', 'Your First'],
        'docs/best-practices.md': ['INITIAL.md', 'Examples', 'PRP Quality'],
        'docs/troubleshooting.md': ['Common Issues', 'Solutions', 'Debug']
    }
    
    for doc, sections in required_sections.items():
        with open(doc, 'r') as f:
            content = f.read()
            for section in sections:
                assert section in content, f"{section} missing from {doc}"

def test_examples_runnable():
    '''Documentation code examples are valid.'''
    import ast
    import re
    
    # Extract Python code blocks
    for doc in Path('docs').rglob('*.md'):
        with open(doc, 'r') as f:
            content = f.read()
            
        # Find Python code blocks
        code_blocks = re.findall(r'```python\n(.*?)\n```', content, re.DOTALL)
        
        for code in code_blocks:
            try:
                ast.parse(code)
            except SyntaxError:
                # Some blocks might be partial - that's OK
                pass
```

### Level 3: Diagram Rendering
```bash
# Test Mermaid diagrams
for diagram in docs/diagrams/*.mermaid; do
  echo "Checking $diagram"
  # Verify basic Mermaid syntax
  grep -E "graph|flowchart|sequenceDiagram" "$diagram"
done
```

## Final validation Checklist
- [ ] README.md enhanced with quick wins section
- [ ] Complete getting-started tutorial created
- [ ] Best practices guide with real examples
- [ ] Troubleshooting covers common issues
- [ ] Advanced patterns documented
- [ ] Integration guides for major platforms
- [ ] Visual diagrams illustrate workflows
- [ ] All documentation cross-referenced
- [ ] Examples are complete and tested
- [ ] FAQ addresses user questions

---

## Anti-Patterns to Avoid
- ❌ Don't write vague documentation
- ❌ Don't show code without context
- ❌ Don't skip the "why" explanations
- ❌ Don't use inconsistent formatting
- ❌ Don't forget to update when code changes
- ❌ Don't assume prior knowledge without explaining