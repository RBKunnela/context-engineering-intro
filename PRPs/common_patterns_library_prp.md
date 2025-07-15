name: "Common Patterns Library PRP"
description: |

## Purpose
Create a comprehensive common_execution_patterns.md library containing reusable patterns for database migrations, service layers, API endpoints, testing, error handling, security, and monitoring that AI assistants can reference during PRP execution.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build a comprehensive library of common execution patterns that provides copy-paste ready implementations for frequently needed components, reducing implementation time and ensuring consistency across PRP executions.

## Why
- Common patterns appear in most PRPs
- Copy-paste patterns reduce errors
- Consistency across implementations
- Faster execution with proven patterns
- Reduces research time for standard components

## What
Create patterns library with:
- Database migration patterns
- Service layer patterns
- API endpoint patterns
- Testing patterns
- Error handling patterns
- Security patterns
- Monitoring patterns
- Integration patterns

### Success Criteria
- [ ] Patterns cover 80% of common needs
- [ ] Each pattern is complete and runnable
- [ ] Patterns follow best practices
- [ ] Easy to find and copy patterns
- [ ] Patterns are well-documented
- [ ] Integration points clear
- [ ] Validation included for each pattern

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: examples/
  why: Existing patterns to incorporate
  
- file: PRPs/EXAMPLE_multi_agent_prp.md
  why: Shows pattern usage in practice
  
- file: CLAUDE.md
  why: Patterns must follow these rules

- file: docs/guides/execute_prp.md
  why: How patterns fit in execution

- file: .claude/commands/execute-prp.md
  why: Pattern usage during execution
```

### Current Codebase tree
```bash
context-engineering-intro/
├── examples/           # Some patterns exist
├── docs/              # Documentation exists
└── (no patterns library)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── patterns/
│   │   ├── common_execution_patterns.md    # Main patterns library
│   │   ├── database_patterns.md           # Database-specific patterns
│   │   ├── api_patterns.md                # API endpoint patterns
│   │   ├── testing_patterns.md            # Test patterns
│   │   └── security_patterns.md           # Security patterns
│   └── reference/
│       └── pattern_index.md               # Quick pattern finder
```

### Known Gotchas
```python
# CRITICAL: Patterns must be generic enough to adapt
# But specific enough to be useful

# CRITICAL: Include imports and setup
# Pattern should work with copy-paste

# CRITICAL: Show both async and sync versions
# Different projects have different needs

# CRITICAL: Include error cases
# Happy path isn't enough
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Create main patterns library:
CREATE docs/patterns/common_execution_patterns.md:
  - ORGANIZE: By category
  - PROVIDE: Complete implementations
  - INCLUDE: Usage notes
  - ADD: Integration guidance

Task 2 - Create database patterns:
CREATE docs/patterns/database_patterns.md:
  - INCLUDE: Migration patterns
  - ADD: Connection pooling
  - SHOW: Transaction handling
  - PROVIDE: ORM patterns

Task 3 - Create API patterns:
CREATE docs/patterns/api_patterns.md:
  - IMPLEMENT: REST endpoints
  - ADD: Authentication
  - SHOW: Rate limiting
  - INCLUDE: Response formatting

Task 4 - Create testing patterns:
CREATE docs/patterns/testing_patterns.md:
  - PROVIDE: Unit test patterns
  - ADD: Integration tests
  - SHOW: Mocking strategies
  - INCLUDE: Fixtures

Task 5 - Create security patterns:
CREATE docs/patterns/security_patterns.md:
  - IMPLEMENT: Auth patterns
  - ADD: Input validation
  - SHOW: Secret management
  - INCLUDE: Security headers

Task 6 - Create pattern index:
CREATE docs/reference/pattern_index.md:
  - INDEX: All patterns
  - TAG: By use case
  - LINK: To implementations
  - SEARCH: Quick finding
```

### Per task pseudocode

```markdown
# Task 1 - common_execution_patterns.md
# Common Execution Patterns Library

## Overview

This library contains battle-tested patterns for common implementation tasks. Each pattern is:
- Complete and runnable
- Well-documented
- Includes error handling
- Has validation tests
- Follows best practices

## Quick Pattern Finder

| Need | Pattern | Location |
|------|---------|----------|
| Database connection | Connection Pool | [Database](#connection-pool) |
| REST API endpoint | FastAPI CRUD | [API](#fastapi-crud) |
| Background task | Celery Worker | [Async](#celery-worker) |
| Auth middleware | JWT Auth | [Security](#jwt-auth) |
| File upload | Streaming Upload | [Files](#streaming-upload) |
| Cache implementation | Redis Cache | [Performance](#redis-cache) |
| Email sending | Email Service | [Communication](#email-service) |
| Webhook handler | Webhook Receiver | [Integration](#webhook-receiver) |

## Database Patterns

### Connection Pool Pattern
```python
"""Database connection pool with retry logic."""

import asyncio
from contextlib import asynccontextmanager
from typing import AsyncGenerator
import asyncpg
from asyncpg import Pool
import backoff

class DatabasePool:
    """Manages database connections with pooling."""
    
    def __init__(self, database_url: str, min_size: int = 10, max_size: int = 20):
        self.database_url = database_url
        self.min_size = min_size
        self.max_size = max_size
        self._pool: Pool | None = None
    
    @backoff.on_exception(
        backoff.expo,
        asyncpg.PostgresConnectionError,
        max_tries=5
    )
    async def connect(self) -> None:
        """Create connection pool with retry."""
        self._pool = await asyncpg.create_pool(
            self.database_url,
            min_size=self.min_size,
            max_size=self.max_size,
            command_timeout=60,
            connection_class=LoggingConnection  # Custom logging
        )
    
    async def disconnect(self) -> None:
        """Close all connections."""
        if self._pool:
            await self._pool.close()
    
    @asynccontextmanager
    async def acquire(self) -> AsyncGenerator[asyncpg.Connection, None]:
        """Acquire connection from pool."""
        if not self._pool:
            await self.connect()
        
        async with self._pool.acquire() as connection:
            yield connection
    
    async def execute(self, query: str, *args, timeout: float = None) -> str:
        """Execute query with automatic connection handling."""
        async with self.acquire() as connection:
            return await connection.execute(query, *args, timeout=timeout)
    
    async def fetch(self, query: str, *args) -> list[asyncpg.Record]:
        """Fetch results with automatic connection handling."""
        async with self.acquire() as connection:
            return await connection.fetch(query, *args)

# Usage Example
db = DatabasePool("postgresql://user:pass@localhost/db")

async def get_user(user_id: int):
    query = "SELECT * FROM users WHERE id = $1"
    result = await db.fetch(query, user_id)
    return result[0] if result else None

# Testing Pattern
async def test_database_pool():
    """Test connection pool functionality."""
    db = DatabasePool("postgresql://test:test@localhost/test")
    
    try:
        # Test connection
        await db.connect()
        
        # Test query
        result = await db.fetch("SELECT 1 as test")
        assert result[0]['test'] == 1
        
        # Test connection reuse
        tasks = [db.fetch("SELECT 1") for _ in range(100)]
        await asyncio.gather(*tasks)
        
    finally:
        await db.disconnect()
```

### Migration Pattern
```python
"""Database migration pattern with rollback support."""

import asyncio
from datetime import datetime
from pathlib import Path
import asyncpg

class Migration:
    """Single migration definition."""
    
    def __init__(self, version: str, name: str, up: str, down: str):
        self.version = version
        self.name = name
        self.up = up
        self.down = down
        self.applied_at: datetime | None = None

class MigrationRunner:
    """Handles database migrations."""
    
    def __init__(self, db_pool: DatabasePool):
        self.db = db_pool
        self.migrations_table = "schema_migrations"
    
    async def initialize(self):
        """Create migrations tracking table."""
        await self.db.execute(f'''
            CREATE TABLE IF NOT EXISTS {self.migrations_table} (
                version VARCHAR(255) PRIMARY KEY,
                name VARCHAR(255) NOT NULL,
                applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        ''')
    
    async def run_migration(self, migration: Migration):
        """Run a single migration."""
        async with self.db.acquire() as conn:
            async with conn.transaction():
                # Run migration
                await conn.execute(migration.up)
                
                # Record migration
                await conn.execute(
                    f"INSERT INTO {self.migrations_table} (version, name) VALUES ($1, $2)",
                    migration.version, migration.name
                )
    
    async def rollback_migration(self, migration: Migration):
        """Rollback a migration."""
        async with self.db.acquire() as conn:
            async with conn.transaction():
                # Run rollback
                await conn.execute(migration.down)
                
                # Remove migration record
                await conn.execute(
                    f"DELETE FROM {self.migrations_table} WHERE version = $1",
                    migration.version
                )
    
    async def get_pending_migrations(self, migrations: list[Migration]) -> list[Migration]:
        """Get migrations that haven't been applied."""
        applied = await self.db.fetch(
            f"SELECT version FROM {self.migrations_table}"
        )
        applied_versions = {row['version'] for row in applied}
        
        return [m for m in migrations if m.version not in applied_versions]

# Usage Example
migrations = [
    Migration(
        version="001",
        name="create_users_table",
        up='''
            CREATE TABLE users (
                id SERIAL PRIMARY KEY,
                email VARCHAR(255) UNIQUE NOT NULL,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        ''',
        down="DROP TABLE users"
    ),
    Migration(
        version="002", 
        name="add_user_status",
        up="ALTER TABLE users ADD COLUMN status VARCHAR(50) DEFAULT 'active'",
        down="ALTER TABLE users DROP COLUMN status"
    )
]

# Run migrations
runner = MigrationRunner(db)
await runner.initialize()

pending = await runner.get_pending_migrations(migrations)
for migration in pending:
    print(f"Running migration {migration.version}: {migration.name}")
    await runner.run_migration(migration)
```

## API Patterns

### FastAPI CRUD Pattern
```python
"""Complete CRUD API pattern with FastAPI."""

from fastapi import APIRouter, Depends, HTTPException, Query
from pydantic import BaseModel, EmailStr
from typing import Optional
from datetime import datetime
import uuid

# Models
class UserBase(BaseModel):
    email: EmailStr
    full_name: str

class UserCreate(UserBase):
    password: str

class UserUpdate(BaseModel):
    email: Optional[EmailStr] = None
    full_name: Optional[str] = None
    password: Optional[str] = None

class UserResponse(UserBase):
    id: uuid.UUID
    created_at: datetime
    updated_at: datetime
    
    class Config:
        from_attributes = True

# Service Layer
class UserService:
    """Business logic for users."""
    
    def __init__(self, db: DatabasePool):
        self.db = db
    
    async def create_user(self, user_data: UserCreate) -> UserResponse:
        """Create new user."""
        user_id = uuid.uuid4()
        
        # Hash password (use bcrypt in production)
        hashed_password = f"hashed_{user_data.password}"
        
        query = '''
            INSERT INTO users (id, email, full_name, password_hash)
            VALUES ($1, $2, $3, $4)
            RETURNING *
        '''
        
        try:
            result = await self.db.fetch(
                query, user_id, user_data.email, 
                user_data.full_name, hashed_password
            )
            return UserResponse(**result[0])
        except asyncpg.UniqueViolationError:
            raise HTTPException(400, "Email already exists")
    
    async def get_user(self, user_id: uuid.UUID) -> UserResponse:
        """Get user by ID."""
        query = "SELECT * FROM users WHERE id = $1"
        result = await self.db.fetch(query, user_id)
        
        if not result:
            raise HTTPException(404, "User not found")
        
        return UserResponse(**result[0])
    
    async def list_users(
        self, 
        skip: int = 0, 
        limit: int = 100,
        search: Optional[str] = None
    ) -> list[UserResponse]:
        """List users with pagination and search."""
        query = "SELECT * FROM users WHERE 1=1"
        params = []
        
        if search:
            query += f" AND (email ILIKE ${len(params)+1} OR full_name ILIKE ${len(params)+1})"
            params.append(f"%{search}%")
        
        query += f" ORDER BY created_at DESC LIMIT ${len(params)+1} OFFSET ${len(params)+2}"
        params.extend([limit, skip])
        
        results = await self.db.fetch(query, *params)
        return [UserResponse(**row) for row in results]
    
    async def update_user(
        self, 
        user_id: uuid.UUID, 
        user_data: UserUpdate
    ) -> UserResponse:
        """Update user."""
        # Build dynamic update query
        update_fields = []
        params = [user_id]
        
        for field, value in user_data.model_dump(exclude_unset=True).items():
            if value is not None:
                update_fields.append(f"{field} = ${len(params)+1}")
                params.append(value)
        
        if not update_fields:
            raise HTTPException(400, "No fields to update")
        
        query = f'''
            UPDATE users 
            SET {', '.join(update_fields)}, updated_at = CURRENT_TIMESTAMP
            WHERE id = $1
            RETURNING *
        '''
        
        result = await self.db.fetch(query, *params)
        if not result:
            raise HTTPException(404, "User not found")
        
        return UserResponse(**result[0])
    
    async def delete_user(self, user_id: uuid.UUID) -> None:
        """Delete user."""
        query = "DELETE FROM users WHERE id = $1 RETURNING id"
        result = await self.db.fetch(query, user_id)
        
        if not result:
            raise HTTPException(404, "User not found")

# API Routes
router = APIRouter(prefix="/users", tags=["users"])

# Dependency
async def get_user_service() -> UserService:
    # In production, use dependency injection
    return UserService(db)

@router.post("/", response_model=UserResponse, status_code=201)
async def create_user(
    user: UserCreate,
    service: UserService = Depends(get_user_service)
):
    """Create new user."""
    return await service.create_user(user)

@router.get("/{user_id}", response_model=UserResponse)
async def get_user(
    user_id: uuid.UUID,
    service: UserService = Depends(get_user_service)
):
    """Get user by ID."""
    return await service.get_user(user_id)

@router.get("/", response_model=list[UserResponse])
async def list_users(
    skip: int = Query(0, ge=0),
    limit: int = Query(100, ge=1, le=1000),
    search: Optional[str] = None,
    service: UserService = Depends(get_user_service)
):
    """List users with pagination."""
    return await service.list_users(skip, limit, search)

@router.patch("/{user_id}", response_model=UserResponse)
async def update_user(
    user_id: uuid.UUID,
    user: UserUpdate,
    service: UserService = Depends(get_user_service)
):
    """Update user."""
    return await service.update_user(user_id, user)

@router.delete("/{user_id}", status_code=204)
async def delete_user(
    user_id: uuid.UUID,
    service: UserService = Depends(get_user_service)
):
    """Delete user."""
    await service.delete_user(user_id)

# Testing Pattern
import pytest
from httpx import AsyncClient

@pytest.mark.asyncio
async def test_user_crud():
    """Test user CRUD operations."""
    async with AsyncClient(app=app, base_url="http://test") as client:
        # Create user
        response = await client.post("/users/", json={
            "email": "test@example.com",
            "full_name": "Test User",
            "password": "secret"
        })
        assert response.status_code == 201
        user_id = response.json()["id"]
        
        # Get user
        response = await client.get(f"/users/{user_id}")
        assert response.status_code == 200
        assert response.json()["email"] == "test@example.com"
        
        # Update user
        response = await client.patch(f"/users/{user_id}", json={
            "full_name": "Updated Name"
        })
        assert response.status_code == 200
        assert response.json()["full_name"] == "Updated Name"
        
        # List users
        response = await client.get("/users/")
        assert response.status_code == 200
        assert len(response.json()) > 0
        
        # Delete user
        response = await client.delete(f"/users/{user_id}")
        assert response.status_code == 204
```

[Continue with more patterns...]

# Task 2 - database_patterns.md
# Database Patterns

## Connection Management

### Basic Connection Pattern
```python
import psycopg2
from contextlib import contextmanager

@contextmanager
def get_db_connection():
    """Simple connection context manager."""
    conn = psycopg2.connect(
        host="localhost",
        database="mydb",
        user="user",
        password="password"
    )
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
    finally:
        conn.close()

# Usage
with get_db_connection() as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM users")
        users = cur.fetchall()
```

[Continue with more database patterns...]
```

### Integration Points
```yaml
PATTERN_LIBRARY_INTEGRATION:
  - used by: AI during execution
  - referenced in: PRPs
  - speeds up: Implementation phase
  
ORGANIZATION:
  - categories: Database, API, Testing, etc.
  - searchable: Pattern index
  - complete: Copy-paste ready
```

## Validation Loop

### Level 1: Pattern Completeness
```bash
# Check pattern count
grep -c "^### " docs/patterns/common_execution_patterns.md
# Expected: 20+ patterns

# Verify code blocks
grep -c "```python" docs/patterns/common_execution_patterns.md
# Expected: 30+ code blocks
```

### Level 2: Pattern Functionality
```python
def test_patterns_valid():
    '''All patterns are valid Python code.'''
    import ast
    import re
    
    with open('docs/patterns/common_execution_patterns.md') as f:
        content = f.read()
    
    # Extract Python code blocks
    code_blocks = re.findall(r'```python\n(.*?)\n```', content, re.DOTALL)
    
    for i, code in enumerate(code_blocks):
        try:
            ast.parse(code)
        except SyntaxError as e:
            pytest.fail(f"Pattern {i} has syntax error: {e}")

def test_patterns_complete():
    '''Patterns include necessary imports.'''
    
    # Check that patterns don't reference undefined names
    # Check that async patterns use proper syntax
    # Verify error handling included
```

## Final validation Checklist
- [ ] Pattern library comprehensive
- [ ] Each pattern complete and runnable
- [ ] Patterns organized by category
- [ ] Index enables quick finding
- [ ] Integration guidance included
- [ ] Error handling in all patterns
- [ ] Both sync and async versions
- [ ] Testing patterns for each

---

## Anti-Patterns to Avoid
- ❌ Don't create incomplete patterns
- ❌ Don't forget error handling
- ❌ Don't omit imports
- ❌ Don't make patterns too specific
- ❌ Don't skip documentation
- ❌ Don't forget validation examples