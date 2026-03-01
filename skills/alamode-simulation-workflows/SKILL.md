---
name: alamode-simulation-workflows
description: This skill should be used when users ask about simulation workflows in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Use this skill for end-to-end execution flow (ALM -> ANPHON) and run sequencing.
- Route to `alamode-inputs-and-modeling` for deep per-tag decisions.
- Route to `alamode-analysis-and-output` for file interpretation after runs complete.

### Triage questions
- Is the target workflow harmonic phonons, RTA conductivity, SCPH, or QHA structural optimization?
- Do harmonic/cubic IFC files already exist, or must they be fitted now?
- Which tutorial path best matches the material class (Si, SrTiO3, BaTiO3, ZnO)?
- Is non-analytic correction required (`BORNINFO`, `NONANALYTIC > 0`)?
- Will run be serial, MPI, or hybrid MPI+OpenMP?

### Canonical workflow
1. Create/validate ALM input for displacement-pattern generation (`MODE = suggest`).
2. Generate displaced structures and run external force calculations.
3. Build `DFSET` and run ALM fitting (`MODE = optimize`) to produce `FCSXML`.
4. Run ANPHON in `phonons` mode for baseline bands/DOS/thermo.
5. For thermal conductivity, fit cubic IFCs and rerun ANPHON with `MODE = RTA`.
6. For strong anharmonicity, run `MODE = SCPH` with valid `KMESH_INTERPOLATE`/`KMESH_SCPH` relation.
7. For SCPH thermo continuation, reuse restart artifacts with same `PREFIX` and consistent SCPH mesh/off-diagonal settings.
8. For structural optimization workflows, enable `RELAX_STR` and related relax/qha controls.

### Minimal working example
```bash
# Canonical Si path (docs/source/tutorial_pages/silicon.rst)
cd example/Si
alm si_alm.in > si_alm.log1                    # suggest patterns
python ../../tools/extract.py --VASP=POSCAR.orig vasprun*.xml > DFSET_harmonic
alm si_alm.in > si_alm.log2                    # optimize IFCs
anphon si_phband.in > si_phband.log            # phonons
ls -1 si222.xml si222.bands
```

```bash
# SCPH path (docs/source/tutorial_pages/sto_scph.rst)
export OMP_NUM_THREADS=1
mpirun -np 4 anphon scph.in > scph.log
grep "conv" scph.log
anphon scph2.in > scph2.log
```

### Pitfalls and fixes
- Discontinuous bands: using supercell vectors in `anphon` `&cell` instead of primitive vectors (`docs/source/faq.rst`).
- SCPH mesh mismatch: `KMESH_INTERPOLATE` not commensurate with harmonic supercell or not subset of `KMESH_SCPH` (`docs/source/tutorial_pages/sto_scph.rst`).
- Missing `BORNINFO` with `NONANALYTIC > 0` invalidates NAC setup (`docs/source/anphondir/inputanphon.rst`).
- MPI/OMP oversubscription: set `OMP_NUM_THREADS` explicitly for MPI jobs.
- Restart mismatch: changing SCPH mesh/off-diagonal flags while reusing restart files leads to inconsistent continuation.
- Known reference caveat: STO SCPH run may not converge at 0 K; do not use 0 K thermo row for decisions (`docs/source/tutorial_pages/sto_scph.rst`).

### Convergence and validation checks
- Harmonic run should produce expected files (`.bands`, `.dos`, `.thermo`) for selected `KPMODE`.
- SCPH logs should show convergence by temperature for production range.
- RTA run should generate `.result` and `.kl` files and update `.result` during execution.
- Cross-check phase-transition workflows by free-energy trends (BaTiO3/ZnO tutorial guidance).

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/anphondir/formalism_anphon.rst`
- `docs/source/tutorial_pages/sto_scph.rst`
- `docs/source/tutorial_pages/silicon.rst`
- `docs/source/tutorial.rst`

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
- `anphon/main.cpp` | Dispatch and mode-level execution sequencing.
- `anphon/scph.cpp` | SCPH iteration/restart behavior.
- `anphon/selfenergy.h` | Anharmonic self-energy path for RTA/SCPH details.
- `anphon/write_phonons.cpp` | File-generation path for run artifacts.
- `anphon/system.cpp` | System setup and mesh-dependent internals.
- `anphon/thermodynamics.cpp` | Thermodynamic output path and aggregation.
- `tools/scph_to_qefc.py` | Post-SCPH FC export route for QE-style follow-on use.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
