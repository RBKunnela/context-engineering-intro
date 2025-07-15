name: "Examples Collection PRP"
description: |

## Purpose
Create a comprehensive examples/ folder with various code patterns that AI assistants can reference and follow when implementing features. These examples are critical for successful context engineering.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build a comprehensive examples/ folder containing real-world patterns for CLI tools, agents, API integrations, testing, and error handling that AI assistants can reference when implementing new features.

## Why
- Examples are CRITICAL for AI assistant success (as stated in README.md)
- AI performs much better when it can see patterns to follow
- Reduces implementation errors and inconsistencies
- Provides concrete patterns for common tasks
- Demonstrates best practices and conventions

## What
Create a well-organized examples/ folder with:
- CLI implementation patterns
- Agent architecture examples
- API integration patterns
- Testing patterns and fixtures
- Error handling demonstrations
- Multi-provider patterns
- Configuration examples

### Success Criteria
- [ ] Examples cover all major patterns mentioned in README.md
- [ ] Each example is complete and runnable
- [ ] Examples include both good patterns and anti-patterns
- [ ] README.md in examples/ explains each example's purpose
- [ ] Examples follow CLAUDE.md conventions

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: README.md
  why: Lists specific example categories needed (CLI, Agent, Testing, etc.)
  
- file: CLAUDE.md
  why: Conventions and patterns that examples must follow
  
- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Shows complexity level expected in examples

- file: INITIAL_EXAMPLE.md
  why: Demonstrates how examples are referenced in feature requests

- file: .claude/commands/generate-prp.md
  why: Understand how examples are used during PRP generation
```

### Current Codebase tree
```bash
context-engineering-intro/
├── examples/          # Currently empty
└── ...
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── examples/
│   ├── README.md              # Explains what each example demonstrates
│   ├── __init__.py           # Makes it a proper Python package
│   ├── cli/
│   │   ├── __init__.py
│   │   ├── basic_cli.py      # Basic CLI with argparse
│   │   ├── advanced_cli.py   # CLI with subcommands and rich output
│   │   └── cli_patterns.md   # Explanation of patterns
│   ├── agents/
│   │   ├── __init__.py
│   │   ├── simple_agent.py   # Basic agent structure
│   │   ├── agent_with_tools.py # Agent with tool integration
│   │   ├── multi_agent.py    # Multiple agents working together
│   │   └── agent_patterns.md # Agent design patterns
│   ├── integrations/
│   │   ├── __init__.py
│   │   ├── api_client.py     # Generic API client pattern
│   │   ├── oauth_flow.py     # OAuth authentication pattern
│   │   ├── webhook_handler.py # Webhook receiver pattern
│   │   └── retry_patterns.py # Retry and rate limiting
│   ├── testing/
│   │   ├── __init__.py
│   │   ├── test_patterns.py  # Unit test patterns
│   │   ├── fixtures.py       # Pytest fixtures examples
│   │   ├── mocking.py        # Mock patterns
│   │   └── conftest.py       # Pytest configuration
│   ├── error_handling/
│   │   ├── __init__.py
│   │   ├── custom_exceptions.py # Custom exception classes
│   │   ├── error_handlers.py # Error handling patterns
│   │   └── logging_setup.py  # Logging configuration
│   └── config/
│       ├── __init__.py
│       ├── settings.py       # Configuration management
│       └── .env.example      # Environment variable template
```

### Known Gotchas
```python
# CRITICAL: Examples must be self-contained but follow project patterns
# Each example should be runnable independently

# CRITICAL: Include both positive and negative examples
# Show what TO DO and what NOT TO DO

# CRITICAL: Examples must follow CLAUDE.md conventions
# Use Python, type hints, docstrings, proper imports

# CRITICAL: Examples should be realistic
# Not toy examples but patterns from real projects
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create examples structure and README:
CREATE examples/README.md:
  - EXPLAIN: Purpose of each subdirectory
  - INDEX: All examples with descriptions
  - USAGE: How to use examples in development

Task 2 - Create CLI examples:
CREATE cli/:
  - IMPLEMENT: Basic and advanced CLI patterns
  - INCLUDE: Argument parsing, output formatting, error handling
  - DEMONSTRATE: Subcommands, configuration, help text

Task 3 - Create Agent examples:
CREATE agents/:
  - IMPLEMENT: Single agent, multi-agent patterns
  - SHOW: Tool integration, state management
  - INCLUDE: Async patterns, error recovery

Task 4 - Create Integration examples:
CREATE integrations/:
  - IMPLEMENT: API client patterns
  - SHOW: Authentication flows, retry logic
  - INCLUDE: Rate limiting, webhook handling

Task 5 - Create Testing examples:
CREATE testing/:
  - IMPLEMENT: Unit test patterns
  - SHOW: Fixtures, mocking, parametrization
  - INCLUDE: Integration test patterns

Task 6 - Create Error handling examples:
CREATE error_handling/:
  - IMPLEMENT: Exception hierarchies
  - SHOW: Graceful error handling
  - INCLUDE: Logging setup

Task 7 - Create Config examples:
CREATE config/:
  - IMPLEMENT: Settings management
  - SHOW: Environment variable handling
  - INCLUDE: Validation patterns
```

### Per task pseudocode

```python
# Task 1 - examples/README.md
"""
# Examples Collection

This directory contains reusable patterns and examples for common implementation tasks.
AI assistants should reference these patterns when implementing new features.

## Directory Structure

### cli/
Command-line interface patterns including:
- `basic_cli.py`: Simple CLI with argparse
- `advanced_cli.py`: Rich CLI with subcommands and formatting

### agents/
Agent architecture patterns including:
- `simple_agent.py`: Basic agent structure with state management
- `agent_with_tools.py`: Tool integration patterns
- `multi_agent.py`: Orchestrating multiple agents

[... continue for each directory ...]

## Usage

When implementing a new feature:
1. Find the relevant example pattern
2. Copy the structure
3. Modify for your specific needs
4. Keep the error handling and validation patterns
"""

# Task 2 - CLI Example (basic_cli.py)
"""
'''Basic CLI pattern with proper structure and error handling.'''

import argparse
import sys
from typing import Optional
import logging

def setup_logging(verbose: bool = False) -> None:
    '''Configure logging based on verbosity.'''
    level = logging.DEBUG if verbose else logging.INFO
    logging.basicConfig(
        level=level,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    )

def create_parser() -> argparse.ArgumentParser:
    '''Create and configure argument parser.'''
    parser = argparse.ArgumentParser(
        description='Example CLI tool',
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog='''
Examples:
  %(prog)s --input data.json --output result.txt
  %(prog)s --verbose --format csv
        '''
    )
    
    parser.add_argument(
        '--input', '-i',
        required=True,
        help='Input file path'
    )
    
    parser.add_argument(
        '--output', '-o',
        default='output.txt',
        help='Output file path (default: %(default)s)'
    )
    
    parser.add_argument(
        '--verbose', '-v',
        action='store_true',
        help='Enable verbose logging'
    )
    
    return parser

def main(args: Optional[list[str]] = None) -> int:
    '''Main entry point.'''
    parser = create_parser()
    parsed_args = parser.parse_args(args)
    
    setup_logging(parsed_args.verbose)
    logger = logging.getLogger(__name__)
    
    try:
        logger.info(f"Processing {parsed_args.input}")
        # Main logic here
        process_file(parsed_args.input, parsed_args.output)
        logger.info("Processing complete")
        return 0
        
    except FileNotFoundError as e:
        logger.error(f"File not found: {e}")
        return 1
    except Exception as e:
        logger.exception("Unexpected error occurred")
        return 2

def process_file(input_path: str, output_path: str) -> None:
    '''Process the input file.'''
    # Implementation here
    pass

if __name__ == '__main__':
    sys.exit(main())
"""

# Task 3 - Agent Example (simple_agent.py)
"""
'''Simple agent pattern with state management and error handling.'''

from typing import Dict, Any, Optional
from dataclasses import dataclass
import logging
from abc import ABC, abstractmethod

logger = logging.getLogger(__name__)

@dataclass
class AgentState:
    '''Agent state container.'''
    conversation_history: list[Dict[str, str]]
    context: Dict[str, Any]
    
    def add_message(self, role: str, content: str) -> None:
        '''Add message to history.'''
        self.conversation_history.append({
            "role": role,
            "content": content
        })

class BaseAgent(ABC):
    '''Base agent class with common functionality.'''
    
    def __init__(self, name: str):
        self.name = name
        self.state = AgentState(
            conversation_history=[],
            context={}
        )
        logger.info(f"Initialized agent: {name}")
    
    @abstractmethod
    async def process(self, input_text: str) -> str:
        '''Process input and return response.'''
        pass
    
    async def run(self, input_text: str) -> str:
        '''Run agent with error handling.'''
        try:
            self.state.add_message("user", input_text)
            response = await self.process(input_text)
            self.state.add_message("assistant", response)
            return response
            
        except Exception as e:
            logger.error(f"Agent {self.name} error: {e}")
            error_response = f"Error: {str(e)}"
            self.state.add_message("error", error_response)
            raise

class SimpleAgent(BaseAgent):
    '''Example implementation of an agent.'''
    
    async def process(self, input_text: str) -> str:
        '''Process the input.'''
        # Example processing logic
        logger.debug(f"Processing: {input_text}")
        
        # Simulate some work
        result = f"Processed: {input_text.upper()}"
        
        return result

# Usage example
async def main():
    agent = SimpleAgent("example_agent")
    response = await agent.run("Hello, world!")
    print(response)
"""

# Task 4 - Integration Example (api_client.py)
"""
'''Generic API client pattern with retry and rate limiting.'''

import asyncio
import aiohttp
from typing import Dict, Any, Optional
import backoff
from datetime import datetime, timedelta
import logging

logger = logging.getLogger(__name__)

class RateLimiter:
    '''Simple rate limiter implementation.'''
    
    def __init__(self, calls: int, period: timedelta):
        self.calls = calls
        self.period = period
        self.call_times: list[datetime] = []
    
    async def acquire(self) -> None:
        '''Wait if necessary to respect rate limit.'''
        now = datetime.now()
        
        # Remove old calls
        cutoff = now - self.period
        self.call_times = [t for t in self.call_times if t > cutoff]
        
        # Wait if at limit
        if len(self.call_times) >= self.calls:
            sleep_time = (self.call_times[0] + self.period - now).total_seconds()
            if sleep_time > 0:
                logger.debug(f"Rate limit hit, sleeping {sleep_time:.2f}s")
                await asyncio.sleep(sleep_time)
        
        self.call_times.append(now)

class APIClient:
    '''Generic API client with best practices.'''
    
    def __init__(
        self,
        base_url: str,
        api_key: Optional[str] = None,
        rate_limit: tuple[int, int] = (10, 60)  # 10 calls per 60 seconds
    ):
        self.base_url = base_url.rstrip('/')
        self.api_key = api_key
        self.rate_limiter = RateLimiter(
            calls=rate_limit[0],
            period=timedelta(seconds=rate_limit[1])
        )
        self.session: Optional[aiohttp.ClientSession] = None
    
    async def __aenter__(self):
        '''Context manager entry.'''
        self.session = aiohttp.ClientSession()
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        '''Context manager exit.'''
        if self.session:
            await self.session.close()
    
    def _get_headers(self) -> Dict[str, str]:
        '''Get request headers.'''
        headers = {
            'Content-Type': 'application/json',
            'User-Agent': 'ExampleAPIClient/1.0'
        }
        
        if self.api_key:
            headers['Authorization'] = f'Bearer {self.api_key}'
        
        return headers
    
    @backoff.on_exception(
        backoff.expo,
        (aiohttp.ClientError, asyncio.TimeoutError),
        max_tries=3,
        max_time=30
    )
    async def request(
        self,
        method: str,
        endpoint: str,
        **kwargs
    ) -> Dict[str, Any]:
        '''Make API request with retry logic.'''
        if not self.session:
            raise RuntimeError("Client not initialized. Use 'async with' context.")
        
        await self.rate_limiter.acquire()
        
        url = f"{self.base_url}/{endpoint.lstrip('/')}"
        headers = kwargs.pop('headers', {})
        headers.update(self._get_headers())
        
        logger.debug(f"{method} {url}")
        
        async with self.session.request(
            method,
            url,
            headers=headers,
            timeout=aiohttp.ClientTimeout(total=30),
            **kwargs
        ) as response:
            response.raise_for_status()
            return await response.json()
    
    async def get(self, endpoint: str, **kwargs) -> Dict[str, Any]:
        '''GET request.'''
        return await self.request('GET', endpoint, **kwargs)
    
    async def post(self, endpoint: str, **kwargs) -> Dict[str, Any]:
        '''POST request.'''
        return await self.request('POST', endpoint, **kwargs)

# Usage example
async def main():
    async with APIClient('https://api.example.com', api_key='secret') as client:
        data = await client.get('/users/123')
        print(data)
"""
```

### Integration Points
```yaml
EXAMPLES_USAGE:
  - referenced in: INITIAL.md files
  - used by: /generate-prp command
  - pattern source for: All new implementations
  
VALIDATION:
  - examples must: Run without errors
  - follow: CLAUDE.md conventions
  - demonstrate: Best practices
```

## Validation Loop

### Level 1: Structure Validation
```bash
# Verify directory structure
find examples -type f -name "*.py" | wc -l
# Expected: 15+ Python files

# Check for README
test -f examples/README.md && echo "README exists"

# Verify __init__.py files
find examples -name "__init__.py" | wc -l
# Expected: 7+ init files
```

### Level 2: Code Quality
```bash
# Lint all examples
ruff check examples/ --fix

# Type check examples
mypy examples/ --ignore-missing-imports

# Expected: No errors
```

### Level 3: Example Execution
```python
# Test that examples are runnable
import subprocess
import sys

def test_cli_example():
    '''Basic CLI example runs without errors.'''
    result = subprocess.run(
        [sys.executable, 'examples/cli/basic_cli.py', '--help'],
        capture_output=True,
        text=True
    )
    assert result.returncode == 0
    assert 'usage:' in result.stdout.lower()

def test_imports():
    '''All examples are importable.'''
    import examples.cli.basic_cli
    import examples.agents.simple_agent
    import examples.integrations.api_client
    # No errors = success
```

## Final validation Checklist
- [ ] All example categories from README.md covered
- [ ] Each example includes comprehensive docstrings
- [ ] Examples follow CLAUDE.md conventions
- [ ] Anti-patterns are clearly marked
- [ ] README.md explains each example's purpose
- [ ] All examples pass linting and type checking
- [ ] Examples are self-contained and runnable
- [ ] Realistic patterns, not toy examples

---

## Anti-Patterns to Avoid
- ❌ Don't create incomplete examples
- ❌ Don't use inconsistent styles across examples
- ❌ Don't forget error handling in examples
- ❌ Don't create examples without docstrings
- ❌ Don't use deprecated patterns
- ❌ Don't create overly complex examples that obscure the pattern