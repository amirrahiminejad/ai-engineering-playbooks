# AI-Assisted Backend Reverse Engineering Playbook

## 1. Purpose

This playbook is designed to analyze an existing backend project when its programming language, framework, and architecture are unknown.

Objectives:

- Identify the project's structure and technologies.
- Recover the system's actual architecture from its source code.
- Identify capabilities, business rules, and execution flows.
- Analyze the data model and internal and external dependencies.
- Document existing behavior and change risks.
- Enable the safe development of new capabilities.
- Prepare a roadmap for architectural improvement and incremental rewriting.

## 2. Working Principles

1. Start with a read-only inspection of the source code.
2. Do not modify application code during discovery and documentation.
3. Infer the programming language, framework, and architecture from available evidence; do not assume any technology or architectural pattern in advance.
4. Distinguish observed facts, runtime-verified behavior, inferences, and unknowns.
5. For important findings, record the relevant file path and class, function, or configuration name.
6. Do not treat file or directory names alone as proof that a capability or architectural pattern exists.
7. Report tests, builds, and runtime behavior as successful only when they have actually been executed and verified.
8. Do not run destructive commands, migrations, data-changing operations, or connect to production without explicit authorization.
9. Do not include secrets, keys, tokens, or personal information in documentation.
10. Work phase by phase. Validate each phase's output before proceeding to the next.
11. Maintain evidence-based documentation and avoid unnecessary descriptions of every individual file and class.
12. If evidence is insufficient, record the item as unknown and specify how it can be investigated.

## 3. Documentation Structure

Store all reverse-engineering documentation under the following path:

```text
docs/
└── reverse-engineering/
    ├── README.md
    ├── 01-repository-inventory.md
    ├── 02-execution-baseline.md
    ├── 03-entry-points.md
    ├── 03-api-catalog.md
    ├── 04-capability-map.md
    ├── 05-data-model.md
    ├── 05-dependency-map.md
    ├── 06-architecture-overview.md
    ├── 07-business-rules.md
    ├── 07-risk-register.md
    ├── 07-test-baseline.md
    ├── 08-validation-report.md
    ├── 10-modernization-roadmap.md
    ├── 10-prioritized-backlog.md
    ├── unknowns.md
    ├── diagrams/
    └── flows/
```

If documentation already exists, inspect it first. Avoid creating duplicate files or overwriting valid information.

## 4. Phase 1: Repository Inventory

### Objective

Identify the technologies, project structure, and application execution method without making prior assumptions about the language or framework.

### Activities

- Inspect the repository structure and key files.
- Identify languages and runtime versions.
- Identify frameworks, dependencies, and build tools.
- Identify application entry points.
- Inspect configuration files and environment variables.
- Identify databases, ORMs, and migration tools.
- Inspect Docker, CI/CD, and deployment files.
- Identify testing tools and generated code.
- Record unknowns and investigation limitations.

### Prompt

# Phase 1: Repository Inventory

Analyze the repository to identify its technology stack and structure without assuming a programming language, framework, or architecture.

Inspect:
- Languages and runtime versions
- Frameworks and dependencies
- Build tools and package managers
- Application entry points
- Configuration files
- Databases, ORM libraries, and migration tools
- Messaging, background jobs, and external integrations
- Tests and test frameworks
- Docker, CI/CD, and deployment configuration
- Generated and vendored code

For each finding, provide evidence using file paths and relevant symbols or configuration keys.

Distinguish confirmed facts, inferences, and unknowns.

Do not install dependencies, execute application code, or modify files.

Create `docs/reverse-engineering/01-repository-inventory.md`.

Include a summary of the technology stack, repository structure, execution entry points, external dependencies, risks, and unresolved questions.

Recommend the next investigation steps based on the detected technologies.

### Acceptance Criteria

The technologies, overall structure, and entry points have been identified, or the unknowns have been documented.

## 5. Phase 2: Build and Runtime Baseline

### Objective

Determine the actual status of the project's build, startup, and tests.

### Activities

- Inspect the application's run instructions.
- Check the required tool versions.
- Identify required configuration and external services.
- Review execution scripts before running them.
- Run the build and tests in an isolated environment with appropriate authorization.
- Check startup and health checks where possible.
- Record errors and runtime environment limitations.

Record each check using one of these statuses:

- `PASS`: Executed and successful.
- `FAIL`: Executed and unsuccessful.
- `BLOCKED`: Not completed because of a dependency or environment limitation.
- `NOT RUN`: Not yet executed.
- `UNKNOWN`: Insufficient evidence is available.

### Prompt

# Phase 2: Execution Baseline

Determine whether the backend can be built, tested, and started safely.

Inspect the build and startup scripts before execution.

With appropriate approval and an isolated environment, verify:

- Required tool and runtime versions
- Dependency availability
- Build or compilation
- Existing automated tests
- Application startup
- Required configuration and external services
- Database connectivity and migration requirements
- Available health checks

Do not connect to production or execute destructive operations.

Record each check as PASS, FAIL, BLOCKED, NOT RUN, or UNKNOWN.

Include relevant error messages without exposing secrets.

Create:

- `docs/reverse-engineering/02-execution-baseline.md`
- `docs/reverse-engineering/unknowns.md`

Distinguish environment failures from application failures. Do not repair application code during this phase.

### Acceptance Criteria

The status of the build, startup, tests, and environment dependencies has been documented.

## 6. Phase 3: Identify Backend Entry Points

### Objective

Identify all paths that initiate the execution of system logic.

### Activities

- Enumerate HTTP routes and endpoints.
- Inspect GraphQL, WebSocket, and RPC interfaces, if present.
- Identify webhooks.
- Identify message consumers and event handlers.
- Identify jobs and scheduled tasks.
- Identify CLI and administrative commands.
- Inspect authentication and authorization.
- Extract the input and output contracts of important endpoints.

### Prompt

# Phase 3: Backend Entry Points

Identify all execution entry points in the backend, including HTTP routes, GraphQL, WebSocket, RPC, webhooks, message consumers, event handlers, scheduled jobs, CLI commands, and internal service interfaces.

For each entry point, document:
- Identifier and trigger
- Method and path, topic, schedule, or command
- Handler file and symbol
- Input and output contracts
- Authentication and authorization
- Main downstream calls
- Data reads and writes
- External side effects
- Error handling
- Related tests
- Verification status and unknowns

Create:

- `docs/reverse-engineering/03-entry-points.md`
- `docs/reverse-engineering/03-api-catalog.md`

Do not assume that a declared route is reachable or functional without runtime evidence.

### Acceptance Criteria

An inventory of entry points and the system's main contracts has been produced.

## 7. Phase 4: Recover Capabilities and Execution Flows

### Objective

Understand the system's actual behavior by tracing business operations from their entry points to their outcomes.

### Activities

1. Identify the backend's main capabilities.
2. Prioritize capabilities by business importance and risk.
3. Trace the code execution path for each important capability.
4. Extract validations, business rules, and state changes.
5. Identify data reads and writes.
6. Inspect external service calls and asynchronous operations.
7. Record transactions, exceptions, retries, and side effects.
8. Document success and failure paths.

### Prompt

# Phase 4: Capability and Execution Flow Recovery

Recover the actual business capabilities and execution flows of the backend.

Create a capability map based on source code, API contracts, tests, and available runtime evidence.

For each important capability, document:
- Business purpose
- Trigger and entry point
- Execution path with file paths and symbols
- Validation and business rules
- State transitions
- Data reads and writes
- Transaction boundaries
- External calls and asynchronous operations
- Side effects and error handling
- Existing tests and verification status
- Unresolved questions

Follow indirect calls and asynchronous processing where possible.

Create:
- `docs/reverse-engineering/04-capability-map.md`
- Relevant flow documents under `docs/reverse-engineering/flows/`

Produce Mermaid sequence diagrams for critical flows. Include only interactions supported by evidence.

Clearly identify incomplete execution paths and missing evidence.

### Acceptance Criteria

Important flows have been documented from their entry points through to the last observable step.

## 8. Phase 5: Analyze the Data Model and Dependencies

### Objective

Identify the data structures and dependencies that affect the system's development and changeability.

### Activities

**Data analysis:**

- Identify databases, tables, collections, and entities.
- Extract relationships, keys, constraints, and indexes.
- Inspect migrations and schema changes.
- Identify transactions.
- Examine data ownership and shared access between modules.
- Inspect cascading deletes and updates.
- Identify sensitive data and how it is used.

**Dependency analysis:**

- Dependencies between packages and modules.
- Direct and indirect function calls.
- Dependencies on shared schemas.
- API and event contracts.
- External services and infrastructure.
- Circular dependencies and tightly coupled components.

### Prompt

# Phase 5: Data and Dependency Analysis

Analyze the backend's data model and dependency relationships.

Identify:
- Databases, tables, collections, and entities
- Relationships, keys, constraints, and indexes
- Migration history
- Transaction boundaries
- Shared database access
- Data ownership and mutation paths
- Module and package dependencies
- Direct and indirect calls
- API and event contracts
- External services and infrastructure dependencies
- Circular dependencies and tightly coupled components

For every important relationship, provide evidence from the repository.

Distinguish declared schema from runtime schema and verified dependencies from inferred ones.

Create:

- `docs/reverse-engineering/05-data-model.md`
- `docs/reverse-engineering/05-dependency-map.md`

Include ER diagrams and dependency diagrams where useful.

Do not execute schema-changing operations.

### Acceptance Criteria

The data model and important dependencies of each capability can be traced.

## 9. Phase 6: Recover the Architecture

### Objective

Create an evidence-based picture of the system's actual architecture.

### Activities

- Draw a System Context diagram.
- Draw a Container diagram.
- Draw Component diagrams for important areas.
- Document internal and external communications.
- Identify module and service boundaries.
- Identify shared dependencies.
- Identify architectural constraints.
- Record differences between the observed architecture and the desired architecture.

### Prompt

# Phase 6: Architecture Recovery

Recover the architecture that actually exists based on evidence gathered in previous phases.

Produce:

- C4 System Context diagram
- C4 Container diagram
- Component diagrams for critical components
- Dependency diagrams
- Data-flow and sequence diagrams for critical operations
- Architectural constraints and risk summary

For each component, document its responsibility, evidence, dependencies, data relationships, and runtime role when verified.

Distinguish observed architecture, inferred architecture, and desired future architecture.

Do not assume layered architecture, Clean Architecture, DDD, or microservices without supporting evidence.

Create:

- `docs/reverse-engineering/06-architecture-overview.md`
- Relevant diagrams under `docs/reverse-engineering/diagrams/`

Describe the actual structure even when it does not match a standard architectural pattern.

### Acceptance Criteria

The system structure and the relationships between its main components are documented with supporting evidence.

## 10. Phase 7: Business Rules, Risks, and Tests

### Objective

Identify sensitive behaviors and determine the level of confidence required before changing them.

### Activities

- Extract business rules from code, tests, and constraints.
- Review access control.
- Examine transactions and data consistency.
- Investigate concurrency and race conditions.
- Investigate idempotency and duplicate requests.
- Examine retries, timeouts, and failures of external services.
- Identify irreversible operations.
- Review test coverage for critical paths.
- Define characterization and integration tests.

### Prompt

# Phase 7: Risk and Test Baseline

Analyze the backend's critical business rules, failure scenarios, and change risks.

Investigate:

- Business invariants
- Authentication and authorization
- Data consistency and transactions
- Concurrent operations and race conditions
- Idempotency and duplicate requests
- External-service failures, retries, and timeouts
- Irreversible operations
- Sensitive-data handling
- Existing test coverage

For each risk, document:
- Identifier
- Description and evidence
- Affected capabilities and components
- Likelihood and impact
- Existing safeguards
- Verification method
- Recommended mitigation
- Priority and status

Propose characterization and integration tests for critical flows.

Create:

- `docs/reverse-engineering/07-business-rules.md`
- `docs/reverse-engineering/07-risk-register.md`
- `docs/reverse-engineering/07-test-baseline.md`

Do not treat missing tests as proof of a defect. Do not change production logic during this phase. Report only test results that have actually been observed.

### Acceptance Criteria

Important rules, risks, existing tests, and testing gaps have been identified.

## 11. Phase 8: Validate the Documentation

### Objective

Ensure that the documentation matches the source code and actual evidence.

### Activities

- Verify that referenced files and symbols exist.
- Compare diagrams with execution paths.
- Check the correctness of relationships and dependencies.
- Identify contradictions between the documentation and the code.
- Distinguish confirmed findings from hypotheses.
- Check for exposed confidential information.
- Verify the accuracy of build and test statuses.
- Record incomplete items and required corrections.

### Prompt

# Phase 8: Documentation Validation

Independently validate all artifacts under `docs/reverse-engineering/`.

Verify:

- Referenced files and symbols exist.
- Evidence supports each important claim.
- Source-level findings are distinguished from runtime verification.
- Diagrams match observed execution paths.
- Architectural labels are supported by evidence.
- Important flows and dependencies are not omitted.
- No secrets or sensitive information are exposed.
- Build and test results are accurate.

Do not silently convert uncertain findings into confirmed facts.

Create `docs/reverse-engineering/08-validation-report.md`.

For every issue, record its severity, affected artifact, supporting evidence, and required correction.

Do not modify application code.

### Acceptance Criteria

The documentation has been reviewed, and incomplete or contradictory items have been identified.

## 12. Phase 9: Develop New Capabilities Safely

### Objective

Use the reverse-engineering results to evolve the product in a controlled manner.

### Activities

1. Define the requirements for the new capability.
2. Locate related flows and business rules.
3. Analyze the change's impact on APIs, data, dependencies, and external services.
4. Identify the required tests.
5. Propose the smallest acceptable solution.
6. After the plan is approved, implement the change within the agreed scope.
7. Run the relevant tests and build.
8. Review the diff and keep the documentation up to date.

### Prompt

# Phase 9: Safe Feature Development

Feature request:\
[DESCRIBE THE FEATURE]

Before modifying code, analyze the existing implementation and reverse-engineering documentation.

Identify:
- Relevant execution flows and business rules
- Existing behavior that must be preserved
- Affected components and dependencies
- Database and schema impacts
- API and event contract changes
- Security and authorization implications
- Transaction, concurrency, and failure-handling concerns
- Required tests
- Expected files to change
- Deployment and rollback considerations

Provide an evidence-based change impact analysis and the smallest safe implementation plan.

Wait for approval before modifying code.

After approval:

1. Implement only the agreed scope.
2. Add or update tests.
3. Execute relevant tests and build.
4. Review the complete diff.
5. Check for unintended behavior changes.
6. Update affected documentation.
7. Report remaining risks and unverified assumptions.

Do not refactor unrelated code or perform a broad rewrite as part of a small feature request.

### Acceptance Criteria

Changes are supported by impact analysis, appropriate tests, and up-to-date documentation.

## 13. Phase 10: Modernization Roadmap

### Objective

Determine which parts should be retained, improved, separated, or replaced.

### Activities

For each area, assess:

- Business importance
- Maintenance cost
- Frequency of change
- Cohesion of responsibilities
- Dependencies
- Data ownership
- Testability
- Replacement risk
- Migration complexity
- Dependencies from other components

Classify each area into one of the following categories:

- Keep and document
- Improve internally
- Establish clear module boundaries
- Extract behind an explicit contract
- Replace incrementally
- Consider a complete rewrite

### Prompt

# Phase 10: Modernization Roadmap

Use verified architecture findings, business flows, risks, and test results to prepare an incremental modernization roadmap.

For each candidate component, evaluate:

- Business importance
- Maintenance cost
- Change frequency
- Coupling and cohesion
- Data ownership
- Reliability and security risks
- Testability
- Replacement cost and risk
- Migration complexity
- Dependencies on other components

Classify each candidate as:
- Keep and document
- Improve internally
- Establish clearer module boundaries
- Extract behind an explicit contract
- Replace incrementally
- Consider a complete rewrite

For each proposed action, document the evidence, expected benefits, alternatives, prerequisites, implementation sequence, risks, verification method, and rollback strategy.

Do not recommend microservices or a complete rewrite solely because the existing code is poorly structured.

Create:
- `docs/reverse-engineering/10-modernization-roadmap.md`
- `docs/reverse-engineering/10-prioritized-backlog.md`

Separate confirmed problems from assumptions and improvement opportunities.

### Acceptance Criteria

The roadmap is based on evidence, risk, and business priorities.

## 14. Finding Template

Use the following template for important findings:

```text
# Finding: [Title]

- ID: FIND-001
- Status: Observed / Runtime-Verified / Inferred / Unknown
- Confidence: High / Medium / Low
- Affected Capability: [Capability]
- Affected Components: [Components]

## Evidence
- File: [Path]
- Symbol: [Class / Function / Configuration Key]

## Observation
[What was actually found]

## Interpretation
[What the evidence suggests]

## Risk or Impact
[Potential consequences]

## Verification Needed
[Additional evidence required]

## Recommended Action
[Next step]

## Related Tests
[Test references]

## Last Verified
[Date or commit reference]
```

## 15. Reverse-Engineering Completion Criteria

- The project's technologies and execution method have been identified.
- Entry points and main contracts have been identified.
- Critical capabilities and flows have been documented.
- The data model and important dependencies have been identified.
- Observed architecture has been distinguished from inferred architecture.
- Important business rules and risks have been recorded.
- The test status of critical flows is known.
- The documentation has been validated against the source code.
- A change-impact analysis process has been defined.
- A modernization roadmap has been prepared.

## Appendix: Require Persian Output

Add the following section at the beginning of prompts when the generated output must be in Persian.

### Language and Output Requirements

Write all documentation, findings, summaries, analysis, risks, and unresolved questions in clear, technically accurate Persian (Farsi).

Preserve the original English names of programming languages, frameworks, libraries, classes, methods, variables, configuration keys, file paths, commands, and technical identifiers.

Keep the documentation filename and directory structure exactly as specified in this prompt.

Use Markdown headings, lists, and tables where appropriate. Technical terms may be written in Persian followed by their English equivalents on first use.

Do not translate or modify source code, identifiers, or configuration values.

These language requirements apply to every Markdown file created or updated during this phase.
