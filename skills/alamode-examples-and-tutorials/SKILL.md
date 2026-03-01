---
name: alamode-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill to choose the right example directory and reproduce a known-good tutorial flow quickly.
- Route to `alamode-inputs-and-modeling` for deep parameter decisions that tutorial snippets do not explain.
- Route to `alamode-analysis-and-output` after tutorial execution when interpreting artifacts.

### Triage questions
- Which material class/workflow is needed: Si baseline, Si anharmonic IFCs, BaTiO3 strongly anharmonic, SrTiO3 SCPH, or ZnO QHA relax?
- Which external engine is used for force generation (VASP/QE/OpenMX/xTAPP/LAMMPS)?
- Is the objective a fast sanity check or production-quality convergence?
- Are reference files being reused or full DFT regeneration required?

### Canonical workflow
1. Select tutorial path from `docs/source/tutorial.rst`.
2. Enter corresponding `example/<material>/...` directory.
3. Run tutorial commands in documented order (pattern generation -> force extraction -> optimize -> anphon run).
4. Reuse `reference/` datasets when the goal is workflow validation rather than recomputing DFT data.
5. Confirm expected key artifacts are produced and match documented file names.
6. Promote the workflow to production by tightening convergence controls and mesh/cutoff checks.

### Minimal working example
```bash
# Si baseline path (docs/source/tutorial_pages/silicon.rst)
cd example/Si
alm si_alm.in > si_alm.log1
python ../../tools/extract.py --VASP=POSCAR.orig vasprun*.xml > DFSET_harmonic
alm si_alm.in > si_alm.log2
anphon si_phband.in > si_phband.log
ls -1 si222.xml si222.bands
```

```bash
# SrTiO3 SCPH path (docs/source/tutorial_pages/sto_scph.rst)
cd example/SrTiO3
cp reference/STO_anharm.xml.bz2 . && bunzip2 STO_anharm.xml.bz2
cp reference/BORN .
mpirun -np 4 anphon scph.in > scph.log
```

### Pitfalls and fixes
- CV file overwrite across sets: use unique `PREFIX` per CV run (`docs/source/tutorial_pages/silicon_ifc.rst`).
- CV aggregation error (`Inconsistent number of entries`): align cvset entry counts or stop-criterion behavior.
- Weakly anharmonic vs strongly anharmonic dataset strategy mismatch: choose Si random-normal-coordinate flow vs BaTiO3 AIMD+random flow appropriately.
- For SCPH tutorials, ensure mesh constraints (`KMESH_INTERPOLATE` subset relation to `KMESH_SCPH`) are respected.
- Tutorial speed options (coarser tolerances/mesh) are for quick runs; production requires explicit convergence checks.

### Convergence and validation checks
- Tutorial reference logs should report small residual/fitting errors for fit stages.
- CV examples should show a clear minimum alpha in `.cvscore`.
- SCPH examples should show convergence over temperature range and document any non-converged points.
- Structural-relax tutorials should produce smooth temperature trends in displacement/strain outputs.

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/tutorial_pages/silicon.rst`
- `docs/source/tutorial_pages/silicon_ifc.rst`
- `docs/source/tutorial_pages/bto_ifc.rst`
- `docs/source/tutorial.rst`
- `docs/source/tutorial_pages/silicon_lammps.rst`

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
- `tools/displace.py` | Shared displacement-generation behavior used across tutorials.
- `tools/extract.py` | Force/displacement extraction path for tutorial DFSET generation.
- `tools/interface/LAMMPS.py` | LAMMPS interface behavior in tutorial workflows.
- `tools/interface/VASP.py` | VASP parser path used by helper scripts.
- `tools/interface/QE.py` | QE parser path used by helper scripts.
- `tools/scph_to_qefc.py` | SCPH output conversion in advanced tutorial chains.
- `tools/taylor.py` | PES/force-constant manipulation helper path.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
