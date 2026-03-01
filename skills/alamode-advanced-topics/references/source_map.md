# alamode source map: Advanced Topics

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" alm anphon external include tools`
- `rg -n "(TODO|FIXME|error|warning|deprecated)" alm anphon tools`

## Suggested source entry points
- `alm/input_parser.cpp` | functions: `InputParser::parse_input`, `InputParser::parse_general_vars`, `InputParser::parse_optimize_vars` | check: `rg -n "InputParser::(parse_input|parse_general_vars|parse_optimize_vars)" alm/input_parser.cpp`
- `alm/optimize.cpp` | functions: `Optimize::optimize_main`, `Optimize::crossvalidation`, `Optimize::run_auto_cv` | check: `rg -n "Optimize::(optimize_main|crossvalidation|run_auto_cv)" alm/optimize.cpp`
- `anphon/parsephon.cpp` | functions: `Input::parse_general_vars`, `Input::parse_scph_vars`, `Input::parse_relax_vars` | check: `rg -n "Input::(parse_general_vars|parse_scph_vars|parse_relax_vars)" anphon/parsephon.cpp`
- `anphon/scph.cpp` | functions: `Scph::setup_scph`, `Scph::exec_scph`, `Scph::load_scph_dymat_from_file` | check: `rg -n "Scph::(setup_scph|exec_scph|load_scph_dymat_from_file)" anphon/scph.cpp`
- `anphon/error.h` | error namespace and messages used by ANPHON runtime failures | check: `rg -n "class|struct|inline|exit\(" anphon/error.h`
- `alm/error.h` | error namespace and messages used by ALM runtime failures | check: `rg -n "class|struct|inline|exit\(" alm/error.h`
- `tools/interface/VASP.py` | methods: `VaspParser.parse`, `VaspParser.parse_or_repair_xml_file` | check: `rg -n "def (parse|parse_or_repair_xml_file)\(" tools/interface/VASP.py`
- `tools/interface/QE.py` | methods: `QEParser.parse`, `QEParser._get_namelist` | check: `rg -n "def (parse|_get_namelist)\(" tools/interface/QE.py`
- `tools/analyze_phonons.cpp` | executable entry point for quick CLI diagnostics | check: `rg -n "^int main\(" tools/analyze_phonons.cpp`
