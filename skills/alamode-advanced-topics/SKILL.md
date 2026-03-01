---
name: alamode-advanced-topics
description: Consolidated advanced and low-frequency topics (theory roots, FAQ, download, scripting/test harness, and doc index) for alamode with docs-first routing and targeted source escalation.
---

# alamode: Advanced Topics

## Scope
- Handle lower-frequency or cross-cutting requests that were previously split into one-doc topic skills.
- Keep routing concise: answer from docs first, then escalate to targeted source files only when behavior details are unresolved.

## Route the request
- Theory/formalism details -> `docs/source/almdir/formalism_alm.rst`.
- ALM/ANPHON documentation-root navigation -> `docs/source/alm_root.rst`, `docs/source/anphon_root.rst`.
- FAQ/debug heuristics -> `docs/source/faq.rst`.
- Download and package acquisition -> `docs/source/download.rst`.
- Docs structure overview -> `docs/source/index.rst`.
- Scripted regression smoke test -> `test/README.md`.
- Docs build requirements -> `docs/requirements.txt`.

## Primary documentation references
- `docs/source/almdir/formalism_alm.rst`
- `docs/source/alm_root.rst`
- `docs/source/anphon_root.rst`
- `docs/source/faq.rst`
- `docs/source/download.rst`
- `docs/source/index.rst`
- `test/README.md`
- `docs/requirements.txt`

## Workflow
- Start with the references above.
- If details are missing, inspect `references/doc_map.md` for full inventory and grouped intent.
- Escalate to `references/source_map.md` only after documentation is exhausted.
- Cite exact file paths in responses for reproducibility.

## Source entry points for unresolved issues
- `alm/input_parser.cpp` | Input parsing semantics tied to formalism-driven constraints.
- `alm/optimize.cpp` | Regression behavior and constraint application internals.
- `anphon/main.cpp` | ANPHON mode dispatch and startup path.
- `anphon/scph.cpp` | SCPH internals for advanced method questions.
- `anphon/error.h` and `alm/error.h` | Error definitions used for troubleshooting traces.
- `tools/interface/VASP.py`, `tools/interface/QE.py`, `tools/interface/OpenMX.py`, `tools/interface/LAMMPS.py`, `tools/interface/xTAPP.py` | Script interface pathways for external engines.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
