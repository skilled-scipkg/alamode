---
name: alamode-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for ALM/ANPHON input schema, tag semantics, and modeling choices that control fit stability/accuracy.
- Route to `alamode-simulation-workflows` for full run orchestration.
- Route to `alamode-analysis-and-output` when the question is output interpretation rather than setup.

### Triage questions
- Which stage is being configured: ALM fitting (`suggest`/`optimize`) or ANPHON run (`phonons`/`RTA`/`SCPH`/`QHA`)?
- Harmonic only, cubic, quartic, or mixed-order IFC model?
- Do you have `DFSET`, and is its line count consistent with `NAT`/`NDATA`?
- Are `FCSXML` and optional `FC2XML`/`FC3XML` basis-compatible (`FCSYM_BASIS`)?
- For SCPH/QHA: what are `KMESH_INTERPOLATE`, `KMESH_SCPH`/`KMESH_QHA`, and structural-relaxation tolerances?

### Canonical workflow
1. Build ALM input with required fields (`&general`, `&interaction`, `&cutoff`, `&cell`, `&position`; add `&optimize` for fitting).
2. Use `MODE = suggest` to generate patterns, then `MODE = optimize` with `DFSET` to fit IFCs.
3. Set model complexity explicitly (`NORDER`, `NBODY`, cutoff radii, optional `LMODEL`/CV tags).
4. Set `FCSYM_BASIS` intentionally and keep it consistent with reference IFC files.
5. Build ANPHON input for target mode; set `FCSXML` (and `FC2XML` if mixed-supercell harmonic/anharmonic use is intended).
6. For SCPH/QHA, configure mesh/tolerance tags and structural-relaxation controls (`RELAX_STR`, `COORD_CONV_TOL`, `CELL_CONV_TOL`, etc.).
7. Validate choices against tutorial patterns for Si/BaTiO3/SrTiO3/ZnO.

### Minimal working example
```ini
# ALM fitting skeleton (docs/source/almdir/inputalm.rst)
&general
  PREFIX = model
  MODE = optimize
  NAT = 64; NKD = 1
  KD = Si
  FCSYM_BASIS = Lattice
/
&interaction
  NORDER = 2
  NBODY = 2 3
/
&optimize
  DFSET = DFSET_harmonic
  CV = 0
/
```

```ini
# ANPHON SCPH skeleton (docs/source/anphondir/inputanphon.rst)
&general
  PREFIX = run
  MODE = SCPH
  FCSXML = STO_anharm.xml
/
&scph
  KMESH_INTERPOLATE = 2 2 2
  KMESH_SCPH = 4 4 4
  SELF_OFFDIAG = 0
  WARMSTART = 1
/
```

### Pitfalls and fixes
- `FCSYM_BASIS = Lattice` changes `.fcs` basis; compare in Cartesian only after conversion; `.xml` stays Cartesian (`docs/source/almdir/inputalm.rst`).
- Rotational-invariance constraints are unsupported with `FCSYM_BASIS = Lattice`; use `Cartesian` if those constraints are needed.
- `FCSYM_BASIS` mismatch with `FC2XML`/`FC3XML` causes hard errors (`docs/source/almdir/inputalm.rst`).
- `NDATA` mismatch from malformed DFSET (line count not divisible by `NAT`) triggers errors.
- For cutoff-based force fields, `FC_ZERO_THR` may need tightening (for example `1e-15`) when generating renormalized harmonic IFC files.
- `KMESH_INTERPOLATE` must be commensurate with harmonic supercell; `KMESH_SCPH` must include it as a subset (`docs/source/tutorial_pages/sto_scph.rst`).
- `WARMSTART = 1` often improves SCPH convergence; overly large `ADD_HESS_DIAG` slows convergence (`docs/source/anphondir/inputanphon.rst`).

### Convergence and validation checks
- CV runs should show a stable minimum in `.cvscore` before locking `L1_ALPHA`.
- Fit logs should report acceptable residual/fitting error for target model/data.
- SCPH logs should show temperature-by-temperature convergence except documented edge cases (e.g., STO at 0 K).
- Structural optimization runs should converge within configured tolerances and produce `atom_disp`/`umn_tensor` trends consistent with tutorial expectations.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/anphondir/inputanphon.rst`
- `docs/source/almdir/inputalm.rst`
- `docs/source/tutorial_pages/zno_qha_relax.rst`
- `docs/source/tutorial_pages/bto_scph_relax.rst`

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
- `alm/input_parser.cpp` | Parsing and validation of ALM input fields/tags.
- `alm/input_setter.cpp` | Tag propagation into ALM model objects.
- `alm/optimize.cpp` | Regression and CV behavior tied to `LMODEL`, `CV`, `L1_ALPHA`.
- `anphon/scph.cpp` | SCPH iteration logic (`KMESH_*`, `SELF_OFFDIAG`, convergence).
- `anphon/qha.cpp` | QHA pathway behavior and mode-dependent controls.
- `anphon/relaxation.cpp` | Structural optimization controls and tolerances.
- `tools/scph_to_qefc.py` | SCPH effective FC export semantics.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
