# Integrate a model into NGIAB

The workshop team has a dummy Python BMI model (regardless of input, every output value is 0) that they would like you to integrate into NGIAB. The model source code is located in a [GitHub repository](https://github.com/quinnylee/bmi_dummy). The model is also published on [PyPI](https://pypi.org/project/bmi-dummy/). An input data package for testing the dummy BMI model within NGIAB is stored in [Box](https://alabama.box.com/s/8sx72ozsg9uxh3e61how7fi0qoolpy1w).

Before starting this section, make sure you have cloned the NGIAB-CloudInfra repository and have installed your containerization software of choice (Docker or Podman).

## Choose your adventure

If you would like to try integrating the model on your own with minimal hints, follow our [surface-level guide](try-it-guides/i-know-docker.md). This is only recommended if you know Docker.

If you would like detailed instructions, follow our [detailed guide](try-it-guides/i-do-not-know-docker.md).

## More resources

Some useful resources:

- [Docker documentation](https://docs.docker.com/)
- [NGIAB 101 training module](https://ngiab.ciroh.org/training-NGIAB-101/)
- [Git submodule documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

Examples:

- [Example realizations](https://github.com/CIROH-UA/NGIAB_data_preprocess/tree/main/modules/data_sources/config/realization)
- [Example BMI configurations](https://github.com/CIROH-UA/NGIAB_data_preprocess/tree/main/modules/data_sources/config/catchment)
- Example pull requests
  - [Adding a Fortran model as submodule to ngen](https://github.com/CIROH-UA/ngen/pull/28)
  - [Building a Fortran model in NGIAB](https://github.com/CIROH-UA/NGIAB-CloudInfra/pull/453)
  - [Adding attributes to community hydrofabric](https://github.com/CIROH-UA/community_hf_patcher/pull/3)
  - [Adding support for a Python model in the Data Preprocessor](https://github.com/CIROH-UA/NGIAB_data_preprocess/pull/195)
  - [Adding support for a Fortran model in the Data Preprocessor](https://github.com/CIROH-UA/NGIAB_data_preprocess/pull/198)
