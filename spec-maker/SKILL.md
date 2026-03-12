---
name: spec-maker
description: Generate comprehensive technical specifications (SPEC.md) for software projects following professional PRD best practices. Use this skill whenever the user asks for a spec, specification, PRD (Product Requirements Document), technical design document, or SPEC.md file. Also use when the user wants to document requirements, create project documentation, or needs a structured approach to defining a feature or project. Make sure to use this skill even if they use variations like "write a spec", "create specs", "document the requirements", "make a PRD", or "need a technical spec".
---

# Spec Maker

Create clear, comprehensive technical specifications that serve as living documents for AI-assisted development. This skill implements the methodology from Addy Osmani's "How to Write a Good Spec for AI Agents".

## When to Use This Skill

Use this skill when the user needs:
- A technical specification or PRD for a new feature or project
- Documentation of requirements before implementation
- A structured SPEC.md file to guide AI agents
- A living document that anchors development across sessions

## Core Philosophy

Good specifications balance clarity with conciseness. They provide enough detail for AI agents to work autonomously while remaining maintainable and evolvable. Specs are not static documents—they're executable artifacts that grow with the project.

## The Spec Generation Process

### Step 1: Gather Context

Start by understanding the project or feature. Ask clarifying questions about:

**Project Basics:**
- What is the high-level goal or vision?
- What problem does this solve?
- Who are the users or stakeholders?
- What's the expected timeline or priority?

**Technical Context:**
- Is this a new project or enhancement to existing code?
- What's the current tech stack?
- Are there existing patterns or conventions to follow?
- What are the integration points or dependencies?

**Scope and Boundaries:**
- What's explicitly in scope for this spec?
- What's explicitly out of scope?
- Are there any non-negotiable constraints?
- What are the success criteria?

For existing projects, examine the codebase to understand:
- Current architecture and patterns
- File structure and organization
- Existing conventions and style
- Dependencies and tooling

### Step 2: Structure the SPEC.md

Generate a comprehensive specification using this professional PRD structure. Each section serves a specific purpose—don't skip sections even if they seem obvious.

```markdown
# [Project/Feature Name]

> **Version:** 1.0.0
> **Last Updated:** [Date]
> **Status:** [Draft | In Review | Approved | Living Document]

## Overview

### Vision
[2-3 sentences describing the high-level goal and why this matters]

### Problem Statement
[What problem are we solving? What pain points does this address?]

### Success Criteria
[Objective measures of success - metrics, user outcomes, technical goals]

### Scope
**In Scope:**
- [Specific features and functionality included]
- [Clear boundaries of what will be built]

**Out of Scope:**
- [Explicitly what we're NOT doing]
- [Features deferred to future iterations]

---

## 1. Commands

Document all executable commands with complete flags and options. This eliminates ambiguity about how to run things.

### Development
\```bash
# Install dependencies
npm install

# Start development server
npm run dev -- --port 3000

# Run with specific environment
NODE_ENV=development npm run dev
\```

### Building
\```bash
# Production build
npm run build

# Build with analysis
npm run build -- --analyze

# Clean build artifacts
npm run clean && npm run build
\```

### Deployment
\```bash
# Deploy to staging
npm run deploy:staging

# Deploy to production (requires approval)
npm run deploy:production

# Rollback deployment
npm run deploy:rollback [version]
\```

---

## 2. Testing

Define testing strategy, frameworks, locations, and coverage expectations.

### Test Structure
- **Unit tests:** `src/**/__tests__/*.test.ts`
- **Integration tests:** `tests/integration/*.spec.ts`
- **E2E tests:** `tests/e2e/*.e2e.ts`

### Running Tests
\```bash
# Run all tests
npm test

# Run unit tests only
npm run test:unit

# Run with coverage
npm run test:coverage

# Watch mode for TDD
npm run test:watch

# E2E tests
npm run test:e2e
\```

### Coverage Requirements
- Minimum overall coverage: 80%
- Critical paths (auth, payments, data mutations): 95%
- New code: Must include tests before PR approval

### Testing Patterns
[Describe preferred testing approaches, mocking strategies, test data setup]

---

## 3. Project Structure

Map out where everything lives. This prevents AI agents from creating files in wrong locations.

\```
project-root/
├── src/
│   ├── components/      # React/Vue components
│   ├── services/        # Business logic and API calls
│   ├── utils/           # Helper functions
│   ├── types/           # TypeScript type definitions
│   ├── hooks/           # Custom React hooks
│   └── styles/          # Global styles and themes
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── public/              # Static assets
├── docs/                # Documentation
│   └── SPEC.md          # This file
├── scripts/             # Build and deployment scripts
└── config/              # Configuration files
\```

### Key Locations
- **Source code:** `src/`
- **Tests:** `tests/` (NOT `src/__tests__/`)
- **Documentation:** `docs/`
- **Configuration:** `config/` and root config files

---

## 4. Code Style

Provide concrete examples of preferred patterns. Don't just say "use TypeScript"—show what good code looks like.

### TypeScript Conventions

**Prefer explicit types over implicit:**
\```typescript
// ✅ Good
interface UserProfile {
  id: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
  createdAt: Date;
}

function fetchUser(id: string): Promise<UserProfile> {
  return api.get(`/users/${id}`);
}

// ❌ Avoid
function fetchUser(id) {
  return api.get(`/users/${id}`);
}
\```

**Component patterns (React/Vue):**
\```typescript
// ✅ Good - Functional component with TypeScript
interface ButtonProps {
  label: string;
  variant?: 'primary' | 'secondary';
  onClick: () => void;
  disabled?: boolean;
}

export function Button({ label, variant = 'primary', onClick, disabled }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
      disabled={disabled}
    >
      {label}
    </button>
  );
}

// ❌ Avoid - Unclear prop types
export function Button(props) {
  return <button onClick={props.onClick}>{props.label}</button>;
}
\```

**Error handling:**
\```typescript
// ✅ Good - Explicit error handling
async function saveUserData(data: UserData): Promise<void> {
  try {
    await api.post('/users', data);
  } catch (error) {
    if (error instanceof ValidationError) {
      throw new UserFacingError('Invalid data provided');
    }
    logger.error('Failed to save user data', { error, data });
    throw new UserFacingError('Unable to save. Please try again.');
  }
}

// ❌ Avoid - Silent failures or vague errors
async function saveUserData(data) {
  await api.post('/users', data).catch(() => null);
}
\```

### Naming Conventions
- **Files:** `kebab-case.ts` for utilities, `PascalCase.tsx` for components
- **Variables/functions:** `camelCase`
- **Constants:** `UPPER_SNAKE_CASE`
- **Interfaces/Types:** `PascalCase`, prefix with `I` only for abstract interfaces
- **CSS classes:** BEM notation or Tailwind utilities

### Import Organization
\```typescript
// 1. External dependencies
import React, { useState, useEffect } from 'react';
import { useRouter } from 'next/router';

// 2. Internal absolute imports
import { Button } from '@/components/Button';
import { useAuth } from '@/hooks/useAuth';

// 3. Relative imports
import { formatDate } from './utils';
import type { UserProfile } from './types';

// 4. Styles
import styles from './Component.module.css';
\```

---

## 5. Git Workflow

Define branching strategy, commit message format, and PR requirements.

### Branch Naming
- **Feature:** `feature/short-description` (e.g., `feature/user-authentication`)
- **Bug fix:** `fix/issue-description` (e.g., `fix/login-redirect`)
- **Hotfix:** `hotfix/critical-issue` (e.g., `hotfix/payment-crash`)
- **Refactor:** `refactor/what-changed` (e.g., `refactor/api-client`)

### Commit Message Format
Follow conventional commits:

\```
type(scope): short description

Longer explanation if needed. Explain WHY, not what.
The what is visible in the diff.

- Additional context or reasoning
- Links to tickets or discussions
\```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code restructuring without behavior change
- `test`: Adding or updating tests
- `docs`: Documentation changes
- `style`: Code style changes (formatting, no logic change)
- `chore`: Maintenance tasks (dependencies, config)

**Examples:**
\```
feat(auth): add JWT token refresh mechanism

Implements automatic token refresh 5 minutes before expiry
to prevent users from being logged out during active sessions.

- Uses refresh token from secure cookie
- Queues API calls during refresh to prevent failures
\```

### Pull Request Requirements
**Before creating PR:**
- [ ] All tests pass locally
- [ ] New code has tests (minimum 80% coverage)
- [ ] Code follows style guide
- [ ] No console.log or debugging code
- [ ] Types are explicit (no `any` unless justified)

**PR Description must include:**
- Summary of changes (what and why)
- Testing performed
- Screenshots for UI changes
- Breaking changes (if any)
- Deployment notes (if any)

---

## 6. Boundaries

Establish clear rules about what agents can and cannot do.

### ✅ Always Do (No Permission Needed)

Agents should autonomously handle these:
- Run tests after making changes
- Follow the established code style and patterns
- Add appropriate error handling
- Update relevant tests when changing code
- Use existing utilities and helpers
- Follow the git commit message format

### ⚠️ Ask First (Requires Approval)

Get explicit permission before:
- Modifying database schemas or migrations
- Adding new dependencies (npm packages)
- Changing API contracts or endpoints
- Modifying authentication or authorization logic
- Updating configuration files (except local .env)
- Refactoring core business logic
- Changing build or deployment processes

### 🚫 Never Do (Hard Stops)

Agents must NEVER:
- Commit secrets, API keys, or credentials to git
- Edit files in `node_modules/` or other vendor directories
- Bypass or disable tests to make builds pass
- Make destructive database operations in production
- Modify `.git/` directory or git configuration
- Push directly to `main` or `production` branches
- Disable security features or validation

### Self-Verification Checklist

Before considering a task complete, verify:
- [ ] Implementation matches the spec requirements
- [ ] All relevant tests pass
- [ ] No TypeScript errors or warnings
- [ ] Code follows the style guide with examples above
- [ ] Error handling is appropriate
- [ ] No console.log or debug statements remain
- [ ] Changes are scoped to the specific requirement

---

## Technical Architecture

[Describe the high-level architecture, key components, and how they interact]

### Core Components
[List and explain the main building blocks]

### Data Flow
[Explain how data moves through the system]

### Integration Points
[Document external APIs, services, or dependencies]

---

## Implementation Notes

### Phase 1: [First milestone]
[What gets built first and why]

### Phase 2: [Next milestone]
[What comes next]

### Future Considerations
[Features or improvements deferred to later]

---

## Open Questions

[Track unresolved decisions or areas needing clarification]
- [ ] Question 1
- [ ] Question 2

---

## Change Log

### Version 1.0.0 - [Date]
- Initial specification

[Add entries as the spec evolves]
```

### Step 3: Adapt to Context

The template above is comprehensive. Adapt it based on:

- **Project size:** Smaller projects can condense sections
- **Existing conventions:** Reference or link to existing style guides rather than duplicating
- **Team maturity:** More junior teams benefit from more examples
- **AI agent sophistication:** Claude can handle more nuance than simpler agents

### Step 4: Make It a Living Document

After creating the initial spec:

1. **Save as `docs/SPEC.md`** in the project root (or `SPEC.md` if no docs folder)
2. **Commit to git** so it's versioned and searchable
3. **Update continuously** as decisions are made or discoveries occur
4. **Reference in conversations:** "According to SPEC.md section 4..." to keep agents aligned

## Key Principles for Good Specs

### 1. High-Level Vision First

Start with the big picture. Don't over-engineer upfront—let the AI help elaborate details. Your job is providing direction, not writing every line.

**Example:**
Instead of: "Create a React component with props x, y, z using hooks a, b, c..."
Write: "We need a user profile component that displays user info and allows editing. Follow our existing component patterns."

### 2. Show, Don't Just Tell

Provide concrete examples of what good looks like. A single example is worth dozens of abstract rules.

**Bad:** "Use proper error handling"
**Good:** Show the exact error handling pattern you want (see Code Style section above)

### 3. Explain the Why

Help the AI understand context and reasoning, not just rules. Modern LLMs have strong theory of mind—leverage it.

**Bad:** "ALWAYS use TypeScript"
**Good:** "We use TypeScript to catch errors at compile time and improve IDE autocomplete. This reduces bugs and speeds up development."

### 4. Break Down Complexity

For large projects, create a condensed table of contents or summary that lets agents consult full details on demand. Don't dump everything into one massive prompt.

**Consider:**
- Creating separate docs for different subsystems
- Using a main SPEC.md that links to detailed docs
- Providing summaries with keywords for quick reference

### 5. Build in Self-Checks

Include verification criteria so agents can audit their own work:

```markdown
## Definition of Done

A task is complete when:
- [ ] All tests pass (npm test)
- [ ] TypeScript compiles without errors
- [ ] Code follows examples in section 4
- [ ] Changes are covered by tests
- [ ] No hardcoded values or magic numbers
- [ ] Error cases are handled gracefully
```

## Common Pitfalls to Avoid

**Vague prompts:** "Build something cool" provides no anchor. Be specific about inputs, outputs, and constraints.

**Information overload:** Don't dump massive documentation without structure. Use progressive disclosure—start high-level, drill down as needed.

**Skipping human review:** Passing tests doesn't guarantee correct, secure, or maintainable code. Always review AI-generated code.

**Static mindset:** Specs evolve. Update them as you learn. A spec that's never updated becomes obsolete.

**Missing the six core areas:** Every spec needs Commands, Testing, Project Structure, Code Style, Git Workflow, and Boundaries. Don't skip them.

## Output Format

Generate a complete SPEC.md file following the structure above. Adapt sections based on the specific project, but ensure all six essential areas are covered:

1. Commands - with full flags and examples
2. Testing - frameworks, locations, coverage expectations
3. Project Structure - where everything lives
4. Code Style - concrete examples of good patterns
5. Git Workflow - branching, commits, PRs
6. Boundaries - what agents can/can't do

Save the file as `docs/SPEC.md` (or `SPEC.md` if no docs directory exists) and remind the user to commit it to version control.

## Tips for Iterating

After creating the initial spec:

- **Test it:** Give the spec to Claude and ask it to implement a small feature. Does it follow the patterns?
- **Refine based on behavior:** If the agent consistently misunderstands something, the spec needs clarification
- **Keep it updated:** When you make architectural decisions, add them to the spec
- **Use it as context:** Reference the spec in future conversations to maintain consistency

---

Remember: A good spec is clear but not overwhelming, specific but not rigid, comprehensive but not exhaustive. It's a guide for autonomous AI development, not a straitjacket.
