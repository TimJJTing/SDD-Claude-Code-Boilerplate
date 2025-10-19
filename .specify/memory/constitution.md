<!--
Sync Impact Report:
Version Change: Initial creation → 1.0.0
Modified Principles: N/A (initial creation)
Added Sections:
  - Core Principles (9 principles total)
  - MCP Usage Guidelines
  - Coding Standards
  - Testing Strategy
  - Debugging Workflow
  - Documentation Standards
  - Repository Management
  - Governance
Removed Sections: N/A (initial creation)
Templates Status:
  ✅ plan-template.md - Reviewed, constitution check section aligns with principles
  ✅ spec-template.md - Reviewed, requirements and success criteria align with principles
  ✅ tasks-template.md - Reviewed, task categorization reflects TDD and test types
Follow-up TODOs:
  - Monitor template consistency as constitution evolves
  - Update Context7 library IDs in MCP usage section as new packages are introduced
-->

# SDD Claude Code Boilerplate Constitution

## Core Principles

### I. Spec-Driven Development with TDD

Development MUST follow a specification-first approach combined with Test-Driven Development:

- Every feature begins with a detailed specification document defining user scenarios, requirements, and success criteria
- Implementation follows the TDD workflow: write tests first → ensure tests fail → implement → verify tests pass → refactor
- Tests are written in order: unit tests (foundation) → integration tests (during component assembly) → E2E tests (final validation)
- Red-Green-Refactor cycle is strictly enforced
- No code is written without a corresponding test, except for trivial configuration or scaffolding

**Rationale**: Specifications provide clarity and shared understanding before implementation begins. TDD ensures code quality, prevents regressions, and serves as living documentation. The layered testing approach (unit → integration → E2E) builds confidence incrementally while catching issues early when they are cheapest to fix.

### II. Clean Code Architecture

Code organization MUST follow clean architecture principles:

- Separation of concerns: models, services, controllers, and interfaces are clearly separated
- Dependencies flow inward: outer layers depend on inner layers, never the reverse
- Business logic is isolated from infrastructure concerns (databases, APIs, frameworks)
- Each module, class, and function has a single, well-defined responsibility
- Domain-driven design patterns are preferred for complex business logic

**Rationale**: Clean architecture enables maintainability, testability, and flexibility. It allows business logic to be tested independently of infrastructure, makes code easier to understand, and facilitates changes to external dependencies without affecting core functionality.

### III. MCP Protocol Adherence

All tool integrations MUST follow Model Context Protocol guidelines and server-specific rules.

**Rationale**: Consistent protocol usage ensures reliable integration with external tools and services while maintaining security and data integrity.

### IV. Documentation-First

Code MUST be self-documenting and supplemented with comprehensive external documentation:

- Every function, class, and file requires JSDoc/docstring with purpose, parameters, return types, and usage examples
- Type annotations are mandatory for all function parameters and return values in JavaScript and Python
- Repository documentation follows the standards defined in `docs/guidelines/documentation.md`
- Documentation is written for both human developers and AI code assistants

**Rationale**: Documentation reduces cognitive load, accelerates onboarding, enables effective AI assistance, and serves as a contract for expected behavior. Self-documenting code combined with external documentation creates a comprehensive knowledge base.

### V. Security-First

Security considerations MUST be embedded in every development decision:

- Credentials, secrets, API keys, and PII are NEVER hardcoded or committed to version control
- External dependencies are vetted for security vulnerabilities before introduction
- Input validation and sanitization are mandatory at all entry points
- Authentication and authorization are implemented following industry best practices
- Security reviews are required for all features handling sensitive data

**Rationale**: Security breaches are costly and damaging. Building security into the development process from the start is far more effective than retrofitting it later.

### VI. Code Quality Standards

Code MUST adhere to consistent quality standards:

- Respect existing coding style and patterns in the codebase
- No hardcoded values; use configuration files or environment variables
- Keep functions simple, focused, and testable (KISS principle)
- Prefer existing solutions in the current tech stack over introducing new dependencies
- New packages MUST be popular, actively maintained, and widely supported
- Follow official documentation and best practices for all technologies used

**Rationale**: Consistent quality standards make code predictable, maintainable, and easier to review. Simplicity reduces bugs and cognitive overhead. Careful dependency management prevents technical debt and maintenance burden.

### VII. Semantic Versioning

All project artifacts MUST follow semantic versioning (MAJOR.MINOR.PATCH):

- MAJOR: Breaking changes or backward-incompatible modifications
- MINOR: New features or enhancements that are backward-compatible
- PATCH: Bug fixes, documentation updates, or minor improvements
- Changesets tool is used to automate version updates and changelog generation
- Version changes are documented with clear rationale

**Rationale**: Semantic versioning provides clear communication about the impact of changes, enables dependency management, and sets expectations for users and developers.

### VIII. Comprehensive Testing

Testing MUST cover unit, integration, and end-to-end scenarios:

- **Unit tests**: Test pure functions and business logic in isolation, no UI rendering, no external dependencies, only test code implemented in this project
- **Integration tests**: Test how components work together with mocked external dependencies, verify user intentions and component behavior in test environment
- **E2E tests**: Test complete user flows in real browser, make real requests only to local/development backend (never production), mock expensive/unreliable third-party APIs
- Focus testing effort on critical paths; avoid excessive coverage of edge cases
- Tests are living documentation of expected behavior

**Rationale**: Different test types serve different purposes. Unit tests provide fast feedback and pinpoint failures. Integration tests catch interaction issues. E2E tests validate complete user journeys. This layered approach maximizes confidence while minimizing test maintenance burden.

### IX. Simplicity and Pragmatism

Development decisions MUST prioritize simplicity and practical value:

- Start with the simplest solution that solves the problem (YAGNI - You Aren't Gonna Need It)
- Complexity must be justified with documented rationale and rejected simpler alternatives
- Avoid premature optimization; optimize only when performance issues are measured
- Favor composition over inheritance
- Prefer explicit over implicit behavior

**Rationale**: Simplicity reduces bugs, makes code easier to understand and modify, and accelerates development. Complexity should only be introduced when there is clear, measurable value that justifies the added cost.

## MCP Usage Guidelines

### GitHub MCP

**Permissions**: Read operations are allowed; write operations (commits, PRs, issue creation) REQUIRE explicit user approval before execution.

**Usage Rules**:
- Use for reading repository information, browsing code, checking issues and PRs
- Always confirm with user before creating commits, branches, issues, or pull requests
- Prefer `search_*` tools for targeted queries; use `list_*` tools for broad retrieval
- Use pagination with batches of 5-10 items to manage context efficiently

### Context7 MCP

**Permissions**: Read-only access to documentation for libraries in the current tech stack.

**Usage Rules**:
- Only query documentation for libraries actively used or being considered for adoption
- When introducing a new package, resolve library ID first and document it in this section for future reference
- Current tech stack library IDs:
  - (To be populated as packages are introduced to the tech stack)

**Workflow**:
1. Use `resolve-library-id` to find the Context7-compatible library ID
2. Use `get-library-docs` with the resolved ID to retrieve documentation
3. Document the library ID in this constitution under "Current tech stack library IDs"

### Serena MCP

**Permissions**: Full access for codebase analysis and symbolic code operations.

**Usage Rules**:
- ALWAYS use Serena tools when coding or analyzing the codebase
- Prefer symbolic tools (`get_symbols_overview`, `find_symbol`, `find_referencing_symbols`) over reading entire files
- Use `search_for_pattern` for targeted pattern searches across files
- Leverage memory files for project-specific knowledge and conventions
- Use symbolic editing tools (`replace_symbol_body`, `insert_after_symbol`, `insert_before_symbol`, `rename_symbol`) for precise modifications

**Workflow**:
1. Start with `get_symbols_overview` to understand file structure
2. Use `find_symbol` with appropriate filters to locate specific code
3. Read symbol bodies only when necessary for the task
4. Make edits using symbolic tools for precision and safety

## Coding Standards

### General Principles

- **Documentation**: Write JSDoc or docstrings for every function, class, and file
- **Type Safety**: Annotate all function parameters and return types (JavaScript/Python)
- **Simplicity**: Keep functions simple and focused (KISS principle)
- **Reusability**: Prefer existing solutions over creating new ones
- **Best Practices**: Consult official documentation and follow recommended patterns
- **Security**: Never commit secrets, API keys, credentials, or PII
- **Style Consistency**: Respect and maintain existing code style and patterns
- **Configuration**: Avoid hardcoded values; use configuration or environment variables

### Dependency Management

- Prefer libraries that are:
  - Popular and widely adopted
  - Actively maintained
  - Well-documented
  - Compatible with existing tech stack
- Document new dependencies in project documentation
- Update Context7 library IDs when introducing new packages
- Keep dependencies up-to-date with security patches

## Testing Strategy

### Test Types and Scope

#### Unit Tests

**Purpose**: Test pure functions and business logic in isolation

**Characteristics**:
- No UI rendering
- No dependencies on other modules
- Test only logic implemented in this project (never test external packages)
- Fast execution
- Pinpoint failures to specific functions

**Example**: Testing a calculation function, validation logic, data transformation

#### Integration Tests

**Purpose**: Test how components, state, and logic work together

**Characteristics**:
- Renders components
- Mocks external dependencies (APIs, services)
- Tests user intentions and component behavior
- Runs in test environment (jsdom), not real browser
- Validates integration between internal modules

**Example**: Testing a form submission flow, state management with components, user interaction sequences

#### E2E Tests

**Purpose**: Test complete user journeys from start to finish

**Characteristics**:
- Runs in real browser
- Tests entire application stack
- Makes real requests to backend ONLY when:
  - Backend is local or development environment
  - Backend does not connect to production database
  - Backend is explicitly prepared for testing
- Validates user experience and fulfillment of specifications

**API Mocking in E2E**: Mock third-party APIs when they:
- Cost money (payment gateways, SMS services)
- Are unreliable or rate-limited
- Are not under our control

**Example**: Complete user registration flow, checkout process, multi-page workflows

### TDD Workflow

For each feature in your specification:

#### 1. Break Spec into Small Units of Logic
- Write unit tests for core logic
- Implement the logic
- Verify tests pass, refactor as needed

#### 2. Build or Import Components Using That Logic
- Write integration tests for component interactions
- Build or connect components
- Verify tests pass

#### 3. Complete Feature
- Write E2E tests for user journeys
- Verify complete user flow works end-to-end

#### Why This Order?

1. **Unit tests first**: Ensures foundation is solid before building on it
2. **Integration tests during**: Catches issues during assembly, maintains focus
3. **E2E tests last**: Validates complete feature, catches integration issues between parts

#### Exception: When to Write E2E Tests Early

If your specification includes critical user journeys that affect architectural decisions, write E2E test shells early (they will fail initially) to:
- Clarify the expected flow
- Guide architectural decisions
- Serve as acceptance criteria

Implement and make them pass last.

### Testing Guidelines

- Focus testing on critical paths and typical user scenarios
- Avoid excessive effort on extremely edge cases
- Tests serve as living documentation
- Keep tests maintainable and readable
- Run tests frequently during development

## Debugging Workflow

When encountering issues or bugs:

### 1. Read Official Documentation
- Use Context7 MCP to retrieve up-to-date library documentation
- Focus search on error messages, concepts, or APIs involved in the issue
- Verify usage patterns match recommended best practices

### 2. Search GitHub Issues
- Use GitHub MCP to search for similar issues in relevant repositories
- Look for known bugs, workarounds, or discussions
- Check both open and closed issues
- If the issue can bd solved with a workaround, notify human developer with known risks and ask for approval before proceeding

### 3. When to Document Issues
- If issue remains unsolvable after steps 1-2, create a bug report
- If issue is solved with a workaround, create a bug report
- Write markdown file under `.issues/` using bug issue template:
  - Bug description
  - How to reproduce
  - Expected behavior
  - Severity
  - Environment
  - How to solve the issue (if known)
  - Additional context (screenshots, references)
- Ask user whether to open an upstream issue

## Documentation Standards

Follow comprehensive documentation guidelines defined in `docs/guidelines/documentation.md`.

### Key Requirements

- **Code Documentation**: JSDoc/docstrings for all functions, classes, files
- **README.md**: Business domain, tech stack, architecture, setup, API usage
- **CLAUDE.md**: LLM-specific guidance with copy-pastable commands
- **Architecture Diagrams**: Mermaid diagrams for system architecture and function topology
- **API Documentation**: Complete endpoint reference with examples
- **Environment Configuration**: Comprehensive variable documentation with examples
- **Dependency Management**: Clear package management workflow for chosen package manager

### Documentation Triggers

Update documentation when:
- Adding new features or APIs
- Modifying configuration or environment variables
- Changing deployment processes or branching strategy
- Adding or updating dependencies
- Implementing new architectural patterns

## Repository Management

### Branch Protection

**Protected Branches**: `main` (and `develop` if Git Flow is used)

**Rule**: NEVER work directly in protected branches. Always create feature, hotfix, or release branches.

### Branching Model (Git Flow)

#### Branch Types

- **`main`**: Production-ready code, protected
- **`develop`**: Integration branch for ongoing development, base for features
- **`feature/{feature-name}`**: New features, branch from `develop`, merge to `develop`
- **`release/{release-name}`**: Release candidates, branch from `develop`, merge to `main` and `develop`
- **`hotfix/{hotfix-name}`**: Critical fixes, branch from `main`, merge to `main` and `develop`

#### Workflow

```
main       ───●──────────────●───────────●────────►
              │             ╱           ╱
release/*  ───┼───────●────●           ●
              │      ╱      ╲         ╱
develop    ───●──●──●──●─────●───────●────────────►
              │         ╲           ╱
feature/*  ───┼───●──●───●──●──────●
```

### Scope Management

- Keep each branch focused on a single, clear purpose
- If feature scope becomes too large or complex, divide it into smaller features
- Consult user when suggesting subdivision of work

### Changesets Workflow

**Purpose**: Automate version updates and changelog generation

**Usage**:
- When developing a feature or hotfix, add a changeset file describing the impact
- Changeset format follows semantic versioning intent (major, minor, patch)
- Use changesets tool to automate version updates when release/hotfix merges to `main`

**Example**:
```bash
# During feature development
npx changeset add
# Select type: minor (new feature)
# Describe change: "Add user authentication with JWT tokens"

# During release
npx changeset version  # Updates version and CHANGELOG
npx changeset publish  # Publishes (if applicable)
```

### Commit Message Template

Follow guidelines in `docs/guidelines/commit-template.md`.

**Format**:
```
<type>: <subject>

<body> (optional)

<footer> (optional)
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Subject**: Imperative mood, lowercase, <50 chars, no period

**Body**: Explain WHAT and WHY, wrap at 72 chars, use bullets for multiple changes

**Footer**: Reference issues (`Closes #123`), breaking changes, co-authors

### Issue Templates

#### Bug Issue Template

- **Bug Description**: Clear description of the issue
- **How to Reproduce**: Step-by-step instructions
- **Expected Behavior**: What should happen
- **Severity**: Critical, High, Medium, Low
- **Environment**: OS, browser, versions, relevant configuration
- **Additional Context** (optional):
  - References to related issues
  - Screenshots or videos
  - Relevant logs or error messages

### Pull Requests

#### When to Create PR

- When a feature branch is complete and ready to merge to `develop` or `main`
- All tests pass
- Documentation is updated to reflect changes
- Code has been reviewed (if team workflow requires it)

#### PR Message Template

**Summary**: Overview of all commits included in this PR, grouped by type if applicable

**Sanity Check Scenarios**: List of specific scenarios to focus on during review:
- Critical user flows to test
- Edge cases to verify
- Configuration changes to validate
- Performance considerations to check
- Security implications to review

**Documentation**: Confirm that documentation has been updated before PR acceptance

#### PR Workflow

1. Create PR from feature/hotfix/release branch to target branch
2. Generate PR message summarizing commits and sanity checks
3. Run automated tests (CI/CD if configured)
4. Conduct code review (if team workflow requires)
5. Update documentation if not already done
6. Merge after approval and passing tests

## Governance

### Constitution Authority

This constitution supersedes all other development practices and guidelines. When conflicts arise between this constitution and other documents, this constitution takes precedence.

### Amendment Process

1. **Proposal**: Document proposed changes with rationale
2. **Review**: Evaluate impact on existing codebase and templates
3. **Approval**: Obtain user/stakeholder approval
4. **Migration Plan**: Create plan for transitioning existing code if needed
5. **Version Update**: Increment constitution version following semantic versioning:
   - **MAJOR**: Backward incompatible governance/principle removals or redefinitions
   - **MINOR**: New principle/section added or materially expanded guidance
   - **PATCH**: Clarifications, wording improvements, non-semantic refinements
6. **Template Sync**: Update all dependent templates (plan, spec, tasks, commands)
7. **Documentation**: Update all references in project documentation

### Compliance Review

- All PRs MUST verify compliance with constitutional principles
- Complexity introduced MUST be justified with documented rationale
- Security considerations MUST be addressed in code reviews
- Testing requirements MUST be met before merge approval
- Documentation MUST be current and accurate

### Runtime Development Guidance

For AI code assistants and developers, runtime guidance is provided through:
- This constitution document (governance and principles)
- Template files in `.specify/templates/` (workflow structure)
- Documentation guidelines in `docs/guidelines/` (standards)
- Memory files in `.specify/memory/` (project-specific knowledge)

**Version**: 1.0.0 | **Ratified**: 2025-10-20 | **Last Amended**: 2025-10-20
