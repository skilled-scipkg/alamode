# alamode source map: Analysis and Output

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" alm anphon external include tools`
- `rg -n "write|dos|thermo|kappa|selfenergy" anphon tools`

## Suggested source entry points
- `anphon/write_phonons.cpp` | functions: `Writes::writePhononBands`, `Writes::writePhononDos`, `Writes::writeThermodynamicFunc`, `Writes::writeKappa` | check: `rg -n "Writes::(writePhononBands|writePhononDos|writeThermodynamicFunc|writeKappa)" anphon/write_phonons.cpp`
- `anphon/phonon_dos.cpp` | functions: `Dos::calc_dos_all`, `Dos::calc_dos`, `Dos::calc_atom_projected_dos` | check: `rg -n "Dos::(calc_dos_all|calc_dos|calc_atom_projected_dos)" anphon/phonon_dos.cpp`
- `anphon/thermodynamics.cpp` | functions: `Thermodynamics::free_energy_QHA`, `Thermodynamics::compute_FE_total`, `Thermodynamics::compute_FE_bubble_SCPH` | check: `rg -n "Thermodynamics::(free_energy_QHA|compute_FE_total|compute_FE_bubble_SCPH)" anphon/thermodynamics.cpp`
- `anphon/mode_analysis.cpp` | functions: `ModeAnalysis::run_mode_analysis`, `ModeAnalysis::print_selfenergy`, `ModeAnalysis::print_spectral_function` | check: `rg -n "ModeAnalysis::(run_mode_analysis|print_selfenergy|print_spectral_function)" anphon/mode_analysis.cpp`
- `tools/plotband.py` | methods: `preprocess_data`, `run_plot` for band-plot behavior | check: `rg -n "def (preprocess_data|run_plot)\(" tools/plotband.py`
- `tools/plotdos.py` | methods: `sum_atom_projected_dos`, `run_plot` for DOS post-processing | check: `rg -n "def (sum_atom_projected_dos|run_plot)\(" tools/plotdos.py`
- `tools/parse_fcsxml.cpp` | CLI entry point for XML IFC extraction | check: `rg -n "^int main\(" tools/parse_fcsxml.cpp`
