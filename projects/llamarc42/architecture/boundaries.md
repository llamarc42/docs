# Boundaries

## Documentation Layer

Responsible for:
- defining truth
- storing artifacts
- defining retrieval policy

Must not:
- contain logic
- depend on runtime behavior

---

## Core Engine

Responsible for:
- context construction
- session management
- retrieval logic
- model interaction

Must not:
- implement UI
- depend on specific surfaces

---

## CLI

Responsible for:
- user interaction
- command parsing
- terminal UX

Must not:
- implement business logic

---

## PowerShell

Responsible for:
- bootstrap workflows
- scripting support

Must not:
- own core logic long-term

---

## API

Responsible for:
- exposing core functionality
- enabling integrations

Must not:
- duplicate logic from core

---

## Model Runtime (Ollama)

Responsible for:
- inference

Must not:
- define system behavior
