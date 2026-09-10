# automation_platform

## 1. Project Overview

`automation_platform` is a generic B2B automation platform designed to be adapted to different companies without rebuilding a custom system for each client.

The objective is to develop a reusable core and make client-specific behavior come mainly from configuration, connectors, mappings and business rules.

Conceptually:

```text
                    GENERIC PLATFORM
                           │
              ┌────────────┼────────────┐
              ↓            ↓            ↓
          Client A      Client B      Client C
              │            │            │
        configuration / connectors / rules
```

The more clients are added, the more of the existing platform should be reusable.

Many business automations can be represented by the same general pipeline:

```text
Input
  ↓
Extraction / Processing
  ↓
Data normalization
  ↓
Business rules
  ↓
Validation
  ↓
Action
  ↓
Audit / Traceability
```

Example:

```text
Invoice received by email
          ↓
Extract invoice information
          ↓
Validate supplier / amount / references
          ↓
Human approval if necessary
          ↓
Create the invoice in the ERP
          ↓
Store execution history
```

The first versions of the project will implement concrete end-to-end workflows rather than trying to build the entire generic platform upfront. Generic abstractions will be introduced progressively when real use cases justify them.


## 2. Target Architecture

The target architecture is organized around a workflow/orchestration layer coordinating specialized components.

```text
                    API / UI
                       │
                       ↓
              Workflow / Orchestration
                /       |        \
               /        |         \
              ↓         ↓          ↓
        AI / Processing  Rules   Connectors
                                   │
                                   ↓
                           External Systems

                       │
                       ↓
                  Data / State
                       │
                       ↓
                 Infrastructure
```

### API / UI

Entry point of the platform.

It allows users or external systems to start workflows, provide information, approve actions and inspect their status.

### Workflow / Orchestration

The central coordinator of the system.

It determines which steps must be executed and in which order, for example:

```text
receive document
      ↓
extract information
      ↓
apply business rules
      ↓
request approval
      ↓
send data to ERP
```

The workflow layer coordinates the other components without containing all their internal logic.

### Business Rules

Contains deterministic business logic.

Examples:

- require human approval above a certain amount;
- reject a document if mandatory information is missing;
- select a workflow depending on the document type.

Client-specific rules should preferably be configurable rather than hard-coded into the generic core.

### AI / Extraction / Processing

Handles operations requiring AI or more complex data processing.

Examples:

- extracting structured information from PDFs;
- classifying emails;
- understanding free text;
- matching entities;
- using LLMs or other ML models.

AI outputs should be validated before they trigger sensitive actions.

### Connectors / Adapters

Interfaces with external systems.

Examples:

```text
Email
SAP
Odoo
Salesforce
SharePoint
SFTP
REST APIs
```

Connectors isolate external technologies from the rest of the platform. Replacing SAP with Odoo, for example, should not require rewriting the entire workflow system.

### Data / State

Stores the persistent state required by the platform:

- workflow executions;
- extracted data;
- validation results;
- errors;
- approvals;
- audit history.

### Infrastructure

Provides the technical services required to run the platform, such as databases, queues, workers, file storage, secrets and deployment infrastructure.

Infrastructure will be introduced progressively according to actual needs.


## 3. Reliability, Security and Scalability

These are important architectural requirements, but they will be implemented progressively rather than upfront.

### Reliability

The platform should eventually support failure recovery, retries, timeouts, idempotency, persistent workflow state, structured logging and complete audit trails.

### Security

Security must be considered from the beginning, including authentication, permissions, secret management, data isolation, encryption and auditability, with European/GDPR constraints in mind.

### Scalability

The project should remain simple initially. The preferred direction is a modular application that can later scale through workers, queues and horizontally scalable components rather than prematurely introducing a complex microservices architecture.


## 4. Development Method

The project is developed through small, reviewable tasks. Codex agents implement these tasks, but the repository — not the conversation history — is the persistent source of truth.

### Project memory

```text
README.md                 Project overview
AGENTS.md                 Instructions every Codex agent must follow
docs/ARCHITECTURE.md      Current architecture
docs/DECISIONS.md         Important decisions and their rationale
docs/DEVELOPMENT.md       Detailed development conventions
TODO.md                   High-level backlog / next steps

docs/tasks/
├── active/<task>.md      Persistent memory for an active task
└── completed/<task>.md   Archived completed tasks
```

There is **no memory file per agent**. The agent is temporary; the task is persistent.

A new Codex conversation can resume a task by reading:

```text
AGENTS.md
    ↓
relevant project documentation
    ↓
docs/tasks/active/<task>.md
```

Important knowledge discovered during a task must be written back to the repository:
- task-specific information → the task file;
- architectural changes → `ARCHITECTURE.md`;
- important technical choices → `DECISIONS.md`.

### Codex workflow

Each Codex agent works on one bounded task.

```text
Define a small task
        ↓
Create docs/tasks/active/<task>.md
        ↓
Start a Codex conversation
        ↓
Codex reads AGENTS.md + relevant docs
        ↓
Codex creates a dedicated branch + worktree
        ↓
Implement + test the task
        ↓
Update task/docs if needed
        ↓
Human reviews and understands the diff
        ↓
CI validation
        ↓
Merge into main
        ↓
Move task file to completed/ and remove worktree
```

`main` should contain only reviewed and validated work.

For significant architectural changes, Codex must explain the proposed change before implementing it.

> **Principle:** Git, documentation and tests preserve project knowledge; Codex agents execute isolated, replaceable tasks.
