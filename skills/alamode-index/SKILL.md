---
name: alamode-index
description: This skill should be used when users ask how to use alamode and the correct generated documentation skill must be selected before going deeper into source code.
---

# alamode Skills Index

## Route the request
- Classify the request into one of the topic skills below.
- Prefer documentation-first answers and only escalate to source inspection if topic docs and maps are insufficient.
- Keep responses workflow-level by default; drill into per-tag/per-file internals only when asked.

## Generated topic skills
- `alamode-getting-started`: First-run setup and quickstart handoff from ALM to ANPHON.
- `alamode-build-and-install`: Dependencies, CMake/Makefile builds, optional HDF5, runtime link setup.
- `alamode-inputs-and-modeling`: ALM/ANPHON input semantics and modeling choices.
- `alamode-simulation-workflows`: End-to-end execution flows for phonons, RTA, SCPH, and relax/QHA paths.
- `alamode-analysis-and-output`: Output artifacts, interpretation, and post-processing checks.
- `alamode-examples-and-tutorials`: Fast path to runnable example directories and reference workflows.
- `alamode-advanced-topics`: Consolidated lower-frequency topics (formalism roots, FAQ, download, scripting/test harness, docs index).

## Documentation-first inputs
- `docs`

## Tutorials and examples roots
- `example`
- `docs/source/tutorial_pages`

## Test roots for behavior checks
- `test`

## Quick simulation bootstrap
```bash
# Fast smoke run with bundled reference IFCs
cd example/Si/reference
anphon si_phband.in > si_phband.log
ls -1 si222.bands
```

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, inspect the same topic skill `references/doc_map.md`.
- If documentation still leaves ambiguity, inspect the same topic skill `references/source_map.md`.
- Use targeted symbol search while inspecting source (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).

## Source directories for deeper inspection
- `alm`
- `anphon`
- `external`
- `include`
- `tools`
