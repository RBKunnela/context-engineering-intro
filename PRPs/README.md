# PRPs Directory

This directory contains Product Requirements Prompts (PRPs) - comprehensive implementation blueprints for the Context Engineering system.

## Available PRPs

### Core Setup PRPs

1. **project_setup_initialization_prp.md**
   - **Purpose**: Set up foundational project structure (PLANNING.md and TASK.md)
   - **Priority**: High - These files are referenced in CLAUDE.md but don't exist
   - **Confidence**: 10/10

2. **examples_collection_prp.md**
   - **Purpose**: Create comprehensive examples/ folder with patterns AI can follow
   - **Priority**: High - Examples are critical for AI success
   - **Confidence**: 9/10

3. **validation_system_prp.md**
   - **Purpose**: Implement validation scripts and framework for quality assurance
   - **Priority**: Medium - Enables self-correction during implementation
   - **Confidence**: 9/10

4. **documentation_enhancement_prp.md**
   - **Purpose**: Extend documentation with tutorials, best practices, and guides
   - **Priority**: Medium - Improves usability and adoption
   - **Confidence**: 9/10

### Execute-PRP System PRPs

5. **execute_prp_guide_implementation_prp.md**
   - **Purpose**: Create comprehensive 5-phase execution methodology guide
   - **Priority**: High - Core execution documentation
   - **Confidence**: 9/10

6. **quick_start_guide_implementation_prp.md**
   - **Purpose**: Create streamlined 6-step fast track execution guide
   - **Priority**: High - Enables rapid PRP execution
   - **Confidence**: 9/10

7. **execution_tracking_template_prp.md**
   - **Purpose**: Create comprehensive tracking templates for execution monitoring
   - **Priority**: Medium - Improves visibility and metrics
   - **Confidence**: 9/10

8. **validation_runner_implementation_prp.md**
   - **Purpose**: Create systematic validation framework with CI/CD integration
   - **Priority**: Medium - Enhances quality assurance
   - **Confidence**: 9/10

9. **common_patterns_library_prp.md**
   - **Purpose**: Create library of reusable implementation patterns
   - **Priority**: High - Speeds up implementation significantly
   - **Confidence**: 9/10

10. **progress_dashboard_prp.md**
    - **Purpose**: Create enhanced progress dashboard with analytics
    - **Priority**: Low - Nice-to-have visibility enhancement
    - **Confidence**: 9/10

## Execution Order

For best results, execute the PRPs in this order:

### Phase 1: Foundation (Required)
1. `project_setup_initialization_prp.md` - Creates essential project files
2. `examples_collection_prp.md` - Provides patterns for all future work
3. `validation_system_prp.md` - Enables quality checking

### Phase 2: Execute-PRP System (Recommended)
4. `execute_prp_guide_implementation_prp.md` - Core execution guide
5. `common_patterns_library_prp.md` - Reusable patterns
6. `quick_start_guide_implementation_prp.md` - Fast execution path
7. `validation_runner_implementation_prp.md` - Advanced validation

### Phase 3: Enhancement (Optional)
8. `documentation_enhancement_prp.md` - Comprehensive docs
9. `execution_tracking_template_prp.md` - Execution metrics
10. `progress_dashboard_prp.md` - Visual progress tracking

## Usage

To execute any PRP:

```bash
/execute-prp PRPs/[prp-name].md
```

For example:
```bash
/execute-prp PRPs/project_setup_initialization_prp.md
```

## Templates

The `templates/` subdirectory contains:
- `prp_base.md` - Base template for creating new PRPs

## Examples

The `EXAMPLE_multi_agent_prp.md` shows a complete PRP for a multi-agent system implementation.