# Architecture Overview

llamarc42 is composed of four primary layers:

## 1. Documentation layer

- global documents
- project documents
- retrieval policy

This is the source of truth.

## 2. Core engine

- path resolution
- artifact loading
- session management
- retrieval logic
- prompt construction
- model interaction

## 3. Runtime layer

- local model runtime (e.g., Ollama)

## 4. Surface layer

- CLI
- PowerShell
- browser UI
- editor integration

## Key principle

All surfaces must use the same core engine.
