# Documentation Directory Structure

This document defines the **directory structure and organization** for all documentation in `/docs`. It is the single source for **where** to put a doc and **what** to call it.

**For writing standards, template, and format rules, see:**

- **`.cursor/rules/documentation.mdc`** — file naming, length, markdown format, diagrams, cross-referencing

## Project Scope

**WIWOKDETOK** is a civic accountability prototype built around Promise Tracker, Bang Jaga AI, and Walk-o-Meter. Documentation should stay simple and practical.

- **Keep it simple** — Focus on how things work and implementation details.
- **Document what exists** — Not enterprise patterns or abstract designs.
- **Practical examples** — Code and usage, not design philosophy.
- **What to document:** Components, data structures, framework usage, features, file organization.
- **Out of scope:** Security docs, high-level architecture, scalability, production deployment.

## Directory Structure

```
docs/
├── .gitignore
├── README.md                     # This file — structure and rules
├── DOCUMENTATION-TEMPLATE.md     # Quick copy-paste template (see .cursor/rules/documentation.mdc)
│
├── architecture/                 # System architecture, design, data flow
│   ├── README.md
│   └── arch-overview.md
│
├── backend/                       # Backend API documentation
│   └── README.md
│
├── data-model/                    # Data structures, types, domain model
│   └── README.md
│
├── database/                      # Data persistence, schemas, migrations
│   └── README.md
│
├── features/                      # Feature-specific docs and user flows
│   └── README.md
│
├── frameworks/                    # Tech stack (frontend, backend, utilities)
│   ├── README.md
│   └── fw-frontend.md
│
├── frontend/                       # Frontend structure, components, routing
│   └── README.md
│
├── testing/                        # Test strategy, coverage, plans
│   └── README.md
│
└── temp/                           # Optional — temporary analysis/planning (not permanent docs)
    └── (analysis-*, planning-*, notes-*, research-*)
```

## Canonical Feature Specs

Feature specifications live in `docs/features/` and use the `feat-*.md` naming convention.

- `docs/features/feat-promise-tracker.md`
- `docs/features/feat-bang-jaga.md`
- `docs/features/feat-walk-o-meter.md`

Legacy top-level files such as `docs/feature-promise-tracker.md` are compatibility stubs only. Update the canonical files in `docs/features/`.

## Directory Purposes and Naming

| Directory       | Purpose                                      | File prefix / naming   |
| --------------- | -------------------------------------------- | ---------------------- |
| `/architecture` | System architecture, components, data flow   | `arch-*.md`            |
| `/backend`      | API endpoints, request/response, auth       | `api-*.md`             |
| `/data-model`   | Types, interfaces, domain data structures    | `data-*.md`            |
| `/database`     | Persistence, schemas, migrations              | `db-*.md`              |
| `/features`     | Feature docs and user flows                  | `feat-*.md`            |
| `/frameworks`   | Tech stack (frontend, backend, utilities)    | `fw-*.md`              |
| `/frontend`     | Frontend structure, components, routing       | `frontend-*.md`        |
| `/testing`      | Test strategy, coverage, flow docs            | `test-*.md`, `flow-*.md` |
| `/temp`         | Temporary analysis/planning (deletable)      | `analysis-`, `planning-`, `notes-`, `research-` |

Each directory has a **README.md** that repeats purpose, naming, and “when to add” — see that README before adding new files there.

## Quick Reference

| Content type   | Directory       | Example files           | Max lines |
| -------------- | --------------- | ----------------------- | --------- |
| System design | `/architecture` | `arch-overview.md`      | 200–400   |
| Data types    | `/data-model`   | `data-types.md`         | 200–400   |
| Tech stack    | `/frameworks`   | `fw-frontend.md`        | 200–400   |
| Frontend impl | `/frontend`     | `frontend-components.md` | 200–400 |
| API reference | `/backend`      | `api-overview.md`       | 200–400   |
| Features      | `/features`     | `feat-<name>.md`        | 200–400   |
| Storage       | `/database`     | `db-overview.md`        | 200–400   |
| Testing       | `/testing`      | `test-plan.md`, `flow-coverage.md` | 200–400 |
| Temp          | `/temp`         | `analysis-*.md`        | N/A       |

Files under 200 lines are fine if the content is complete. For all standards and the doc template, see `.cursor/rules/documentation.mdc`.
