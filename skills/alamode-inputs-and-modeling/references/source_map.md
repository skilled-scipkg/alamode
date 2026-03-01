# alamode source map: Inputs and Modeling

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "(NORDER|NBODY|FCSYM_BASIS|DFSET|KMESH_|RELAX_STR|L1_ALPHA|CV)" alm anphon`
- `rg -n "parse_|set_|optimize|scph|qha|relax" alm anphon tools/scph_to_qefc.py`

## Suggested source entry points
- `alm/input_parser.cpp` | functions: `InputParser::parse_general_vars`, `InputParser::parse_interaction_vars`, `InputParser::parse_cutoff_radii`, `InputParser::parse_optimize_vars` | check: `rg -n "InputParser::(parse_general_vars|parse_interaction_vars|parse_cutoff_radii|parse_optimize_vars)" alm/input_parser.cpp`
- `alm/input_setter.cpp` | functions: `InputSetter::set_general_vars`, `InputSetter::set_interaction_vars`, `InputSetter::set_optimize_vars` | check: `rg -n "InputSetter::(set_general_vars|set_interaction_vars|set_optimize_vars)" alm/input_setter.cpp`
- `alm/optimize.cpp` | functions: `Optimize::optimize_main`, `Optimize::crossvalidation`, `Optimize::compute_alphas` | check: `rg -n "Optimize::(optimize_main|crossvalidation|compute_alphas)" alm/optimize.cpp`
- `anphon/parsephon.cpp` | functions: `Input::parse_general_vars`, `Input::parse_scph_vars`, `Input::parse_qha_vars`, `Input::parse_relax_vars` | check: `rg -n "Input::(parse_general_vars|parse_scph_vars|parse_qha_vars|parse_relax_vars)" anphon/parsephon.cpp`
- `anphon/scph.cpp` | functions: `Scph::setup_kmesh`, `Scph::update_frequency`, `Scph::exec_scph` | check: `rg -n "Scph::(setup_kmesh|update_frequency|exec_scph)" anphon/scph.cpp`
- `anphon/qha.cpp` | functions: `Qha::setup_qha`, `Qha::exec_qha_optimization`, `Qha::exec_perturbative_QHA` | check: `rg -n "Qha::(setup_qha|exec_qha_optimization|exec_perturbative_QHA)" anphon/qha.cpp`
- `anphon/relaxation.cpp` | functions: `Relaxation::setup_relaxation`, `Relaxation::update_cell_coordinate`, `Relaxation::check_str_divergence` | check: `rg -n "Relaxation::(setup_relaxation|update_cell_coordinate|check_str_divergence)" anphon/relaxation.cpp`
- `tools/scph_to_qefc.py` | methods: `get_dfc2`, `create_newfc2` for effective-FC export behavior | check: `rg -n "def (get_dfc2|create_newfc2)\(" tools/scph_to_qefc.py`
