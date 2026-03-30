# Principles

These principles guide the design and behavior of llamarc42.

## 1. Documentation is the source of truth

The system does not "know" the project.

It relies on:
- global documents
- project documents

All responses must defer to documented information.

---

## 2. Local-first by default

The system must:
- function without external services
- prioritize privacy and control
- operate predictably in offline environments

---

## 3. Conversations are contextual, not authoritative

Conversation history provides working context.

Documentation provides truth.

When conflicts arise, documentation wins.

---

## 4. Transparency over abstraction

- inputs to the model must be inspectable
- session data must be readable
- no hidden or implicit state

---

## 5. Retrieval over training

- project knowledge is retrieved, not baked into the model
- documentation evolves independently of model weights
- behavior may be tuned, but truth remains external

---

## 6. Consistency across surfaces

All interfaces must:
- interpret documentation the same way
- use the same session model
- follow the same retrieval rules

---

## 7. Bounded intelligence over autonomy

The system prioritizes:
- guided assistance
- constrained automation

Over:
- fully autonomous agents

---

## 8. Explicit tradeoffs over generic advice

The system should:
- explain reasoning
- highlight tradeoffs
- avoid absolute or generic recommendations

---

## 9. Simplicity of storage and structure

- files over databases where possible
- readable formats over opaque ones
- minimal required infrastructure

---

## 10. Evolve through usage

The system should be:
- shaped by real workflows
- refined through dogfooding
- validated against actual engineering tasks
