# alamode source map: Skills Index

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting topic-level docs/maps from routed skills.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" alm anphon external include tools`
- `rg -n "(MODE|FCSXML|DFSET|SCPH|RTA|QHA|kpoint)" alm anphon`

## Suggested source entry points
- `alm/alm_cui.cpp` | function: `ALMCUI::run` for ALM CLI entry behavior | check: `rg -n "ALMCUI::run" alm/alm_cui.cpp`
- `anphon/phonons.cpp` | functions: `PHON::execute_phonons`, `PHON::execute_RTA`, `PHON::execute_self_consistent_phonon` | check: `rg -n "PHON::(execute_phonons|execute_RTA|execute_self_consistent_phonon)" anphon/phonons.cpp`
- `alm/input_parser.cpp` | functions: `InputParser::parse_input`, `InputParser::parse_optimize_vars` | check: `rg -n "InputParser::(parse_input|parse_optimize_vars)" alm/input_parser.cpp`
- `anphon/parsephon.cpp` | functions: `Input::parse_general_vars`, `Input::parse_scph_vars`, `Input::parse_qha_vars` | check: `rg -n "Input::(parse_general_vars|parse_scph_vars|parse_qha_vars)" anphon/parsephon.cpp`
- `tools/displace.py` | method: `displace` used across displacement workflows | check: `rg -n "def displace\(" tools/displace.py`
- `tools/extract.py` | method: `run_parse` used across DFSET extraction workflows | check: `rg -n "def run_parse\(" tools/extract.py`
