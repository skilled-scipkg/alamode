# alamode source map: Simulation Workflows

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "(suggest|optimize|phonons|RTA|SCPH|QHA|RELAX_STR|RESTART|KPMODE)" alm anphon`
- `rg -n "(execute_|exec_|compute_kappa|writePhonon|postprocess)" anphon`

## Suggested source entry points
- `alm/input_parser.cpp` | functions: `InputParser::run`, `InputParser::parse_displacement_and_force_files` | check: `rg -n "InputParser::(run|parse_displacement_and_force_files)" alm/input_parser.cpp`
- `alm/optimize.cpp` | functions: `Optimize::optimize_main`, `Optimize::write_cvscore_to_file` | check: `rg -n "Optimize::(optimize_main|write_cvscore_to_file)" alm/optimize.cpp`
- `anphon/phonons.cpp` | functions: `PHON::execute_phonons`, `PHON::execute_RTA`, `PHON::execute_self_consistent_phonon` | check: `rg -n "PHON::(execute_phonons|execute_RTA|execute_self_consistent_phonon)" anphon/phonons.cpp`
- `anphon/scph.cpp` | functions: `Scph::exec_scph`, `Scph::postprocess`, `Scph::store_scph_dymat_to_file` | check: `rg -n "Scph::(exec_scph|postprocess|store_scph_dymat_to_file)" anphon/scph.cpp`
- `anphon/conductivity.cpp` | functions: `Conductivity::calc_anharmonic_imagself`, `Conductivity::compute_kappa` | check: `rg -n "Conductivity::(calc_anharmonic_imagself|compute_kappa)" anphon/conductivity.cpp`
- `anphon/qha.cpp` | functions: `Qha::exec_qha_optimization`, `Qha::exec_perturbative_QHA` | check: `rg -n "Qha::(exec_qha_optimization|exec_perturbative_QHA)" anphon/qha.cpp`
- `anphon/relaxation.cpp` | functions: `Relaxation::setup_relaxation`, `Relaxation::update_cell_coordinate` | check: `rg -n "Relaxation::(setup_relaxation|update_cell_coordinate)" anphon/relaxation.cpp`
- `anphon/write_phonons.cpp` | functions: `Writes::writePhononBands`, `Writes::writeThermodynamicFunc`, `Writes::writeKappa` | check: `rg -n "Writes::(writePhononBands|writeThermodynamicFunc|writeKappa)" anphon/write_phonons.cpp`
- `tools/scph_to_qefc.py` | methods: `parse_QEfc`, `create_newfc2` for SCPH follow-up workflows | check: `rg -n "def (parse_QEfc|create_newfc2)\(" tools/scph_to_qefc.py`
