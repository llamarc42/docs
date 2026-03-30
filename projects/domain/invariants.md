# Invariants

These rules must always hold true.

## Documentation

- Documentation is authoritative over conversation history
- Global documents are always included
- Project documents define project-specific truth

## Sessions

- A session belongs to exactly one project
- Sessions are append-only
- Session history must be preserved

## Retrieval

- Retrieval policy defines what is in scope
- The model must not infer missing project facts
- If context is insufficient, the system must say so

## Behavior

- The system must respect documented constraints and decisions
- The system must prefer explicit tradeoffs over generic advice
- The system must not override architectural intent

## Consistency

- All surfaces (CLI, UI, editor) must behave consistently
- No surface may reinterpret documentation differently

## Transparency

- All inputs to the model must be inspectable
- All outputs must be traceable to inputs
