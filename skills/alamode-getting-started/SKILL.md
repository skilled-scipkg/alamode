---
name: alamode-getting-started
description: This skill should be used when users ask about getting started in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill for first-run setup, first ALM/ANPHON execution, and quick workflow bootstrapping (`docs/source/quickstart.rst`, `docs/source/intro.rst`).
- Route to `alamode-build-and-install` for compiler/dependency/build failures.
- Route to `alamode-inputs-and-modeling` for deep tag semantics (`MODE`, `NBODY`, `FCSYM_BASIS`, `KMESH_*`).
- Route to `alamode-analysis-and-output` for interpreting generated files after successful runs.

### Triage questions
- Are `alm` and `anphon` already built and runnable in the current shell?
- Which external engine is used for forces (VASP/QE/OpenMX/xTAPP/LAMMPS)?
- Is the current task harmonic phonons, RTA conductivity, or SCPH/QHA?
- Do you already have a valid `DFSET` and `FCSXML` file?
- Are you using primitive vectors in `anphon` input and supercell vectors in `alm` input?

### Canonical workflow
1. Confirm SCF convergence for the primitive cell before phonon work (`docs/source/quickstart.rst`).
2. Create `alm` input with `MODE = suggest`, then run `alm` to generate displacement patterns.
3. Generate displaced structures (`tools/displace.py`) and run external force calculations.
4. Build `DFSET` with `tools/extract.py`.
5. Switch `alm` to `MODE = optimize` and fit IFCs to produce `PREFIX.xml`.
6. Create `anphon` input with `MODE = phonons` and `FCSXML = PREFIX.xml`.
7. Run `anphon` to produce bands/DOS/thermo outputs.
8. Plot and sanity-check outputs (`tools/plotband.py`, `tools/plotdos.py`).

### Minimal working example
```bash
# Quickstart command skeleton (docs/source/quickstart.rst)
alm alm.in > alm.log
anphon anphon.in > anphon.log
# or MPI
mpirun -np 4 anphon anphon.in > anphon.log
```

```ini
# Minimal ALM/ANPHON handoff
&general
  PREFIX = si222
  MODE = optimize
/
&optimize
  DFSET = DFSET_harmonic
/
# anphon.in
&general
  PREFIX = si222
  MODE = phonons
  FCSXML = si222.xml
/
```

### Pitfalls and fixes
- Large fitting error: subtract residual-force offset during DFSET creation (`tools/extract.py --offset ...`) and tighten base structure relaxation (`docs/source/faq.rst`).
- Discontinuous phonon bands at BZ boundaries: use primitive vectors in `anphon` `&cell` (`docs/source/faq.rst`).
- Wrong force parsing from QE: ensure `tprnfor=.true.` before extraction (`docs/source/tutorial_pages/silicon.rst`).
- Overly short coordinates degrade symmetry handling: keep high precision fractional coordinates (`docs/source/faq.rst`).
- Bad displacement magnitude: start around `0.01 Angstrom` for harmonic IFCs (`docs/source/quickstart.rst`, `docs/source/tutorial_pages/silicon.rst`).

### Convergence and validation checks
- `alm` optimize run should emit `PREFIX.fcs` and `PREFIX.xml`.
- Harmonic fitting errors are typically low (often a few percent in tutorial flows; `docs/source/faq.rst`).
- Acoustic modes near Gamma should be close to zero in `.bands`.
- Expected outputs for quickstart should include `.bands`, `.dos`, `.thermo` (`docs/source/quickstart.rst`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/quickstart.rst`
- `docs/source/intro.rst`

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
- `alm/main.cpp` | CLI flow for ALM run stages (`suggest`/`optimize`).
- `anphon/main.cpp` | CLI flow for `phonons`, `RTA`, and `SCPH` execution.
- `tools/displace.py` | Displacement generation logic for supported external engines.
- `tools/extract.py` | DFSET construction and offset handling.
- `tools/plotband.py` | Band post-processing assumptions and units.
- `tools/plotdos.py` | DOS plotting paths and options.
- `anphon/symmetry_core.cpp` | Symmetry handling that affects setup/debug outcomes.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
