---
name: alamode-analysis-and-output
description: This skill should be used when users ask about analysis and output in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Analysis and Output

## High-Signal Playbook
### Route conditions
- Use this skill when the user already has ALM/ANPHON outputs and needs interpretation, QA, or post-processing.
- Route to `alamode-simulation-workflows` if output files are missing because upstream execution is incomplete.
- Route to `alamode-inputs-and-modeling` if output problems trace to tag/model configuration.

### Triage questions
- Which mode produced the files (`phonons`, `RTA`, `SCPH`, `QHA`, ALM fitting)?
- Which artifact is needed (`.bands`, `.dos`, `.thermo`, `.result`, `.kl`, `.scph_*`, `.fcs`, `.xml`)?
- What `KPMODE` was used, and were optional analysis flags enabled (`PDOS`, `PRINTMSD`, `PRINTVEL`, etc.)?
- Is the user asking for plotting, physical interpretation, or consistency checks?

### Canonical workflow
1. Identify run mode and map expected outputs using ALM/ANPHON output reference docs.
2. Confirm required files are present for that mode and `KPMODE`.
3. Plot spectra (`plotband.py`, `plotdos.py`) or extract values from text outputs.
4. For RTA, inspect `.result` and `.kl`/`.kl_spec`/`.kl_coherent` as needed.
5. For SCPH thermo, use the correct free-energy column convention from tutorial notes.
6. For structural optimization workflows, inspect `atom_disp`, `normal_disp`, and `umn_tensor` trends vs temperature.

### Minimal working example
```bash
# Common post-processing commands
python tools/plotband.py STO222_NA3.bands
python tools/plotdos.py --emax 550 --nokey si222.dos
head -n 10 STO_scph2-2.scph_thermo
```

```text
# SCPH thermo columns (docs/source/tutorial_pages/sto_scph.rst)
col 3: QHA-like term
col 4: SCPH correction term
col 5: total vibrational free energy to use for phase-stability analysis
```

### Pitfalls and fixes
- Using wrong free-energy column for SCPH: use column 5 for the thermodynamically consistent value (`docs/source/tutorial_pages/sto_scph.rst`).
- Reading 0 K SCPH thermo when SCPH did not converge there: treat as invalid for decisions.
- Expecting DOS in line-mode runs: `.dos` needs `MODE = phonons` with `KPMODE = 2` (`docs/source/anphondir/outputanphon.rst`).
- Poor DOS resolution: increase mesh density and reduce `DELTA_E` (`docs/source/tutorial_pages/silicon.rst`).
- Missing optional files (`.msd`, `.phvel`, `.gru`) because analysis flags were not enabled.

### Convergence and validation checks
- ALM optimize should output both `.fcs` and `.xml`; CV outputs (`.cvset*`, `.cvscore`) should exist when CV is enabled.
- Band outputs should keep acoustic branches near zero at Gamma and avoid obvious discontinuities from setup mistakes.
- RTA outputs should include `.result` plus conductivity tensors in `.kl`.
- SCPH outputs should include `.scph_dymat`/`.scph_bands` and optionally `.scph_thermo`/`.scph_msd` according to enabled flags.

## Scope
- Handle questions about output formats, analysis, and post-processing.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/anphondir/outputanphon.rst`
- `docs/source/almdir/outputalm.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `example`
- `docs/source/tutorial_pages`

## Test references
- `test`

## Optional deeper inspection
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

## Source entry points for unresolved issues
- `anphon/write_phonons.cpp` | Serialization paths for many ANPHON outputs.
- `anphon/mode_analysis.cpp` | Mode-level analysis and related output fields.
- `anphon/phonon_dos.cpp` | DOS generation internals.
- `anphon/thermodynamics.cpp` | Thermodynamic quantity generation.
- `tools/plotband.py` | Band plotting assumptions and unit handling.
- `tools/plotdos.py` | DOS plotting and options.
- `tools/parse_fcsxml.cpp` | XML extraction behavior when debugging IFC content.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
