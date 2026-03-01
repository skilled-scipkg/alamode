# alamode source map: Examples and Tutorials

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "(pattern|displacement|parse|extract|random_normalcoordinate|SCPH|QHA)" tools anphon`
- `rg -n "def (parse|generate_structures|get_displacements|run_parse|displace)\(" tools/*.py tools/interface/*.py`

## Suggested source entry points
- `tools/displace.py` | methods: `check_code_options`, `check_displace_options`, `displace` | check: `rg -n "def (check_code_options|check_displace_options|displace)\(" tools/displace.py`
- `tools/extract.py` | methods: `check_options`, `run_parse` for DFSET generation behavior | check: `rg -n "def (check_options|run_parse)\(" tools/extract.py`
- `tools/GenDisplacement.py` | methods: `generate`, `_parse_displacement_patterns`, `_get_random_displacements_normalcoordinate` | check: `rg -n "def (generate|_parse_displacement_patterns|_get_random_displacements_normalcoordinate)\(" tools/GenDisplacement.py`
- `tools/interface/VASP.py` | methods: `VaspParser.parse`, `VaspParser.get_displacements` | check: `rg -n "def (parse|get_displacements)\(" tools/interface/VASP.py`
- `tools/interface/QE.py` | methods: `QEParser.parse`, `QEParser._get_coordinates_pwout` | check: `rg -n "def (parse|_get_coordinates_pwout)\(" tools/interface/QE.py`
- `tools/interface/LAMMPS.py` | methods: `LammpsParser.parse`, `LammpsParser._print_displacements_and_forces` | check: `rg -n "def (parse|_print_displacements_and_forces)\(" tools/interface/LAMMPS.py`
- `tools/scph_to_qefc.py` | methods: `parse_QEfc`, `get_dfc2`, `create_newfc2` for SCPH-to-QE FC conversion | check: `rg -n "def (parse_QEfc|get_dfc2|create_newfc2)\(" tools/scph_to_qefc.py`
- `anphon/parsephon.cpp` | functions: `Input::parse_general_vars`, `Input::parse_kpoints` for tutorial-input validation | check: `rg -n "Input::(parse_general_vars|parse_kpoints)" anphon/parsephon.cpp`
