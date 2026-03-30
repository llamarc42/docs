# Constraints

## Architectural constraints

- Documentation is the source of truth
- Global documents are always in scope
- Project documents define project-specific behavior
- Sessions are scoped to a single project
- Conversations are append-only

## Runtime constraints

- Must work with local model runtimes (e.g., Ollama)
- Must function offline
- Must not require external services

## Implementation constraints

- PowerShell is a bootstrap surface, not the final core
- Core logic must be portable and cross-platform
- Retrieval must be explicit and controllable

## Product constraints

- Must support multiple surfaces:
  - terminal
  - browser
  - editor

- All surfaces must share:
  - the same session model
  - the same retrieval rules
  - the same interpretation of documentation

## Data constraints

- Sessions must be inspectable on disk
- No hidden or opaque state
- Artifacts must remain human-readable

## Behavioral constraints

- The system must not invent project-specific facts
- The system must defer to documentation over general knowledge
