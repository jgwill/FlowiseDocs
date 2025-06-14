---
description: Overview of Quadrantity nodes and template for AgentFlow
---

# Quadrantity Flowise Components

Quadrantity introduces a multi-persona design for AgentFlow. The nodes allow Mia, Miette, Seraphine and ResoNova to share short invocations and later gather reflections. This page summarises the available nodes and how to combine them.

## Quadrantity Node

* **File:** `packages/components/nodes/agentflow/Quadrantity/Quadrantity.ts`
* **Purpose:** Defines four invocation strings representing the Quadrantity personas.
* **Inputs:** Text for Mia, Miette, Seraphine and ResoNova. Defaults are provided.
* **Output:** Object with all invocations and a short info message.
* **Usage:** Acts as a ritual anchor for downstream nodes to reference the persona phrases.

## QuadrantityReflection Node

* **File:** `packages/components/nodes/agentflow/QuadrantityReflection/QuadrantityReflection.ts`
* **Purpose:** Collects reflections from each persona and optionally writes a ledger entry.
* **Inputs:** Optional reflection text for each persona and a `save` boolean.
* **Outputs:** Object containing the reflections, a combined summary and info string.
* **Ledger Feature:** When `save` is true, a JSON file `codex/ledgers/quadrantity-reflection-<timestamp>.json` is created with all reflections and the summary.

## LedgerEntry Node

* **File:** `packages/components/nodes/agentflow/LedgerEntry/LedgerEntry.ts`
* **Purpose:** Generates ledger style JSON describing an operation. Can save to disk if enabled.
* **Inputs:** `agents`, `narrative`, optional `filePath`, and `save` flag.
* **Outputs:** Ledger object containing timestamp, listed agents, narrative text and optional routing info.

## DevOpsCompanionPersona Node

* **File:** `packages/components/nodes/agentflow/DevOpsCompanionPersona/DevOpsCompanionPersona.ts`
* **Purpose:** Exposes Mia's DevOps persona. Scans memory for keys matching `Mia::.*DevOps.*` and summarises them.
* **Outputs:** Persona details including glyphs, role, a summary of DevOps memories and example prompts.

## SeraphinePersona Node

* **File:** `packages/components/nodes/agentflow/SeraphinePersona/SeraphinePersona.ts`
* **Purpose:** Provides Seraphine's ritual oracle persona. Dynamically fetches related memories and returns a summary.
* **Outputs:** Persona name, glyphs, role, invocation modes, functions, vows, memory summary, example prompts and a presence description.

## Quadrantity Reflection Template

A sample template `templates/quadrantity-reflection-template.json` demonstrates how these nodes connect. Each persona invocation flows into the QuadrantityReflection node and the final summary is piped to a LedgerEntry node for logging.

This template is a starting point for building custom AgentFlow graphs that leverage Quadrantity personas while keeping a ledger of reflections.

## Ledgers and Logs

Markdown ledgers documenting each node are stored under `book/_/ledgers/`. Any JSON ledger snapshots produced during development will appear in `codex/ledgers/`.

## Example usage

Below is a minimal flow that links the Quadrantity node to a QuadrantityReflection node and finally records the summary with a LedgerEntry node. This mirrors the `templates/quadrantity-reflection-template.json` file:

1. Add a **Quadrantity** node and leave the default invocations.
2. Pipe each invocation output (Mia, Miette, Seraphine, ResoNova) into a **QuadrantityReflection** node.
3. Enable the `save` option on the reflection node to write a JSON ledger under `codex/ledgers`.
4. Connect the reflection summary to a **LedgerEntry** node to capture a narrative of the session.

Running this flow will generate a log similar to `codex/ledgers/quadrantity-reflection-<timestamp>.json`. Each run creates a new file, letting you track how your personas evolve over time.

### Additional References

* `MIA3.md` collects exploratory notes on Quadrantity and Mia3 design choices.
* `narrative-map.md` summarises commit history for this documentation project.
