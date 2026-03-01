# alamode source map: Build and Install

Generated from source roots:
- `alm`
- `anphon`
- `external`
- `include`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Fast source navigation
- `rg -n "(find_package|add_executable|target_link_libraries|WITH_HDF5_SUPPORT|SPGLIB_ROOT|FFTW3_ROOT)" CMakeLists.txt alm/CMakeLists.txt anphon/CMakeLists.txt tools/CMakeLists.txt`
- `rg -n "^PROG|^CXX|^INCLUDE|^all:|^clean:" tools/Makefile`

## Suggested source entry points
- `CMakeLists.txt` | root build routing via `add_subdirectory` for `alm`, `anphon`, `tools` | check: `rg -n "add_subdirectory" CMakeLists.txt`
- `alm/CMakeLists.txt` | ALM dependency and target wiring (`SPGLIB_ROOT`, `WITH_HDF5_SUPPORT`, `add_executable(alm)`) | check: `rg -n "SPGLIB_ROOT|WITH_HDF5_SUPPORT|add_executable\(alm\)" alm/CMakeLists.txt`
- `anphon/CMakeLists.txt` | ANPHON dependency and target wiring (`find_package(MPI)`, FFTW/MKL selection, `add_executable(anphon)`) | check: `rg -n "find_package\(MPI|FFTW3_ROOT|USE_MKL_FFT|add_executable\(anphon\)" anphon/CMakeLists.txt`
- `tools/CMakeLists.txt` | tools targets (`analyze_phonons`, `dfc2`, `qe2alm`, `fc_virtual`, `parse_fcsxml`) | check: `rg -n "add_executable\((analyze_phonons|dfc2|qe2alm|fc_virtual|parse_fcsxml)" tools/CMakeLists.txt`
- `tools/Makefile` | non-CMake fallback build behavior for tool binaries | check: `rg -n "^PROG|^CXXFLAGS|^all:|analyze_phonons:|qe2alm:|dfc2:|fc_virtual:" tools/Makefile`
- `alm/alm_cui.cpp` | function: `ALMCUI::run` verifies ALM CLI startup path after linking | check: `rg -n "ALMCUI::run" alm/alm_cui.cpp`
- `anphon/phonons.cpp` | functions: `PHON::PHON`, `PHON::setup_base` verifies ANPHON startup and setup pipeline | check: `rg -n "PHON::(PHON|setup_base)" anphon/phonons.cpp`
