# alamode source map: Getting Started

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "(MODE|FCSXML|DFSET|kpoint|phonons|RTA|SCPH)" alm anphon`
- `rg -n "def (displace|run_parse|run_plot)\(" tools/displace.py tools/extract.py tools/plotband.py tools/plotdos.py`

## Suggested source entry points
- `alm/alm_cui.cpp` | function: `ALMCUI::run` for top-level ALM CLI behavior | check: `rg -n "ALMCUI::run" alm/alm_cui.cpp`
- `alm/input_parser.cpp` | functions: `InputParser::run`, `InputParser::parse_input`, `InputParser::get_run_mode` | check: `rg -n "InputParser::(run|parse_input|get_run_mode)" alm/input_parser.cpp`
- `alm/optimize.cpp` | functions: `Optimize::optimize_main`, `Optimize::least_squares`, `Optimize::crossvalidation` | check: `rg -n "Optimize::(optimize_main|least_squares|crossvalidation)" alm/optimize.cpp`
- `anphon/parsephon.cpp` | functions: `Input::parce_input`, `Input::parse_general_vars`, `Input::parse_kpoints` | check: `rg -n "Input::(parce_input|parse_general_vars|parse_kpoints)" anphon/parsephon.cpp`
- `anphon/phonons.cpp` | functions: `PHON::execute_phonons`, `PHON::execute_RTA`, `PHON::execute_self_consistent_phonon` | check: `rg -n "PHON::(execute_phonons|execute_RTA|execute_self_consistent_phonon)" anphon/phonons.cpp`
- `tools/displace.py` | method: `displace` for pattern-to-structure generation | check: `rg -n "def displace\(" tools/displace.py`
- `tools/extract.py` | method: `run_parse` for DFSET extraction and offsets | check: `rg -n "def run_parse\(" tools/extract.py`
- `tools/plotband.py` | methods: `preprocess_data`, `run_plot` for first-look dispersion checks | check: `rg -n "def (preprocess_data|run_plot)\(" tools/plotband.py`
- `tools/plotdos.py` | method: `run_plot` for DOS quick checks | check: `rg -n "def run_plot\(" tools/plotdos.py`
