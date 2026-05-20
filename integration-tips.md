# Integration tips

This document is a compilation of some general guidelines for community model integration that we have discovered and developed through experience.

## Technical requirements

### All models

For integration of all models, you will need to have these elements ready:

- Example input data package
- Instructions on how to generate forcings and BMI configuration files
- Instructions on how to access source data for forcings and attributes, if necessary

### Python models

Because of the potential for package dependency conflicts with existing Python models in NGIAB, all new Python models need to meet these requirements.

- Compatible with Python 3.11
- `netcdf==1.63` if `netcdf` is used
- `pydantic<2` if `pydantic` is used
- `pandas<3` if `pandas` is used
- PyPI distributions needed for all models and non-standard dependencies (to make wheel building easier)

### Non-Python models

All non-Python models must be compilable against `libc 2.34` or lower.

## General integration workflow

The pull requests mentioned in this section can be made by you or (upon request) by a CIROH Science Team member. To request assistance from CIROH Science Team members, please open an issue in the CIROH-UA/NGIAB-CloudInfra repository for an NGIAB request or in the CIROH-UA/ngen-datastream repository for an NRDS request. Some examples of these workflows are in the [Examples section](#examples).

### NextGen/NGIAB

#### Python models

- PyPI distribution installed into NGIAB Docker image (PR into CIROH-UA/NGIAB-CloudInfra repo)

#### Non-Python models

- Add model as submodule in ngen (PR into CIROH-UA/ngen repo)
- Build model's shared object file in NGIAB Docker image (PR into CIROH-UA/NGIAB-CloudInfra repo)

### Data Preprocessor

PRs into CIROH-UA/NGIAB_data_preprocess will generally involve editing these files/directories.

#### Adding configs and realizations

- `modules/data_processing/create_realization.py`
- `modules/data_sources/config/catchment`
- `modules/data_sources/config/realization`

#### Editing forcing generation

- `modules/data_processing/forcings.py`

#### Adding model as option for CLI

- `modules/ngiab_data_cli/__main__.py`
- `modules/ngiab_data_cli/arguments.py`

### NRDS

- Edit config, forcing, and realization generation (PR into CIROH-UA/forcingprocessor repo)
- Add execution schedules (PR into CIROH-UA/ngen-datastream repo)

## Examples

- [Example realizations](https://github.com/CIROH-UA/NGIAB_data_preprocess/tree/main/modules/data_sources/config/realization)
- [Example BMI configurations](https://github.com/CIROH-UA/NGIAB_data_preprocess/tree/main/modules/data_sources/config/catchment)
- Example pull requests
  - [Adding a Fortran model as submodule to ngen](https://github.com/CIROH-UA/ngen/pull/28)
  - [Building a Fortran model in NGIAB](https://github.com/CIROH-UA/NGIAB-CloudInfra/pull/453)
  - [Adding attributes to community hydrofabric](https://github.com/CIROH-UA/community_hf_patcher/pull/3)
  - [Adding support for a Python model in the Data Preprocessor](https://github.com/CIROH-UA/NGIAB_data_preprocess/pull/195)
  - [Adding support for a Fortran model in the Data Preprocessor](https://github.com/CIROH-UA/NGIAB_data_preprocess/pull/198)
- Example Science Team requests
  - [Requesting assistance for NGIAB integration](https://github.com/CIROH-UA/NGIAB-CloudInfra/issues/385)
  - [Requesting assistance for NRDS integration](https://github.com/CIROH-UA/ngen-datastream/issues/337)
  - [Requesting IT resources for NRDS implementation](https://github.com/CIROH-UA/NGIAB-CloudInfra/issues/448)
