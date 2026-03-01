---
name: alamode-build-and-install
description: This skill should be used when users ask about build and install in alamode; it prioritizes documentation references and then source inspection only for unresolved details.
---

# alamode: Build and Install

## High-Signal Playbook
### Route conditions
- Use this skill for dependency checks, CMake/Makefile builds, and runtime loader issues (`docs/source/install.rst`, `docs/source/install_withHDF5.rst`).
- Route to `alamode-getting-started` once binaries are available and the user needs first run guidance.
- Route to `alamode-advanced-topics` for FAQ-style non-build runtime behavior.

### Triage questions
- Conda toolchain or native toolchain?
- Need optional HDF5 support now, or plain build first?
- Which compiler/MPI stack is expected (gcc/clang/intel; OpenMPI/MPICH/etc.)?
- Are `SPGLIB_ROOT`, `FFTW3_ROOT`, and optional `HDF5_ROOT` known?
- Is the issue configure-time, compile-time, or runtime linking (`LD_LIBRARY_PATH`)?

### Canonical workflow
1. Install mandatory dependencies (compiler, LAPACK, MPI, Boost, Eigen3, spglib, FFTW unless MKL FFT path is used).
2. Clone ALAMODE and enter repository root.
3. Create out-of-tree build directory and run CMake at top level.
4. Build all binaries with `make -j` (or targeted binary such as `make alm -j`).
5. For HDF5-enabled builds, add `-DWITH_HDF5_SUPPORT=yes` and provide `HDF5_ROOT` if auto-detection fails.
6. If CMake is not used, build `alm/`, `anphon/`, and `tools/` with sample Makefiles.
7. Export runtime library paths before execution when needed (`SPGLIB_ROOT`, optional `HDF5_ROOT`).

### Minimal working example
```bash
# CMake path (docs/source/install.rst, docs/source/install_withHDF5.rst)
mkdir -p _build && cd _build
cmake -DUSE_MKL_FFT=no -DSPGLIB_ROOT=$CONDA_PREFIX ..
# Optional HDF5 support:
# cmake -DUSE_MKL_FFT=no -DWITH_HDF5_SUPPORT=yes -DSPGLIB_ROOT=$SPGLIB_ROOT -DHDF5_ROOT=$HDF5_ROOT ..
make -j
```

```bash
# Makefile fallback path
export SPGLIB_ROOT=/path/to/spglib
cd alm && cp Makefile.linux Makefile && make -j
cd ../anphon && cp Makefile.linux Makefile && make -j
cd ../tools && make -j
```

### Pitfalls and fixes
- `cannot find -lsymspg`: pass `-DSPGLIB_ROOT=...` to CMake and export `LD_LIBRARY_PATH` for runtime (`docs/source/install.rst`).
- CMake cannot find Boost/Eigen/FFTW/HDF5: pass `-DBOOST_INCLUDE`, `-DEIGEN3_INCLUDE`, `-DFFTW3_ROOT`, `-DHDF5_ROOT` as needed (`docs/source/install*.rst`).
- Wrong compiler picked in conda env: re-activate environment before rerunning CMake (`docs/source/install.rst`).
- Built with HDF5 but runtime fails to load HDF5 libs: include `$HDF5_ROOT/lib` in `LD_LIBRARY_PATH` (`docs/source/install_withHDF5.rst`).
- Using old branch assumptions: docs show `develop` checkout; keep branch choice explicit in reproducible build notes.

### Convergence and validation checks
- Confirm `_build/alm/alm`, `_build/anphon/anphon`, and tool binaries exist after build.
- Run a minimal command from quickstart/examples to verify executable startup.
- Check CMake configure output for detected compiler/MPI/LAPACK and optional HDF5 status.
- For Makefile builds, validate each subdirectory binary independently (`./alm/alm`, `./anphon/anphon`, `./tools/analyze_phonons`).

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/install_withHDF5.rst`
- `docs/source/install.rst`
- `docs/source/tutorial_pages/pbte_nonanalytic_correction.rst`

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
- `CMakeLists.txt` | Top-level build graph (`alm`, `anphon`, `tools`).
- `alm/CMakeLists.txt` | ALM dependency checks (`spglib`, optional HDF5, LAPACK).
- `anphon/CMakeLists.txt` | ANPHON dependency checks (MPI, FFTW/MKL, optional HDF5).
- `tools/CMakeLists.txt` | Tool binary targets and Boost include behavior.
- `tools/Makefile` | Non-CMake fallback build logic and compiler flags.
- `alm/main.cpp` | Startup path to verify linked runtime behavior.
- `anphon/main.cpp` | Startup path for MPI/OpenMP runtime diagnostics.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" alm anphon external include tools`).
