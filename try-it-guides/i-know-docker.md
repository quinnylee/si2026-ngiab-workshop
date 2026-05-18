# Surface-level guide

## Reminders

The workshop team has a dummy Python BMI model (regardless of input, every output value is 0) that they would like you to integrate into NGIAB. The model source code is located in a [GitHub repository](https://github.com/quinnylee/bmi_dummy). The model is also published on [PyPI](https://pypi.org/project/bmi-dummy/). An input data package for testing the dummy BMI model within NGIAB is stored in [Box](https://alabama.box.com/s/8sx72ozsg9uxh3e61how7fi0qoolpy1w).

The requirements for a Python model to be integrated into NGIAB are as follows:

- Must be compatible with Python 3.11
- Dependency versions as follows:
  - `netCDF==1.6.3`
  - `pydantic<2`
  - `pandas<3`
- PyPI distributions available for all models and non-standard dependencies

## Instructions

1. Please make sure your computer meets the requirements in our ["Before the Workshop" guide](../before-the-workshop.md).
2. Check the model source code to make sure that it meets the requirements for integration into NGIAB.
3. Navigate to the directory where you cloned the NGIAB source code. NGIAB is built with the Dockerfile located in `docker/Dockerfile`.
4. Optional: build the NGIAB image without any changes to speed up any future rebuilds.
5. Edit the Dockerfile to make sure that the bmi-dummy Python package is accessible in the final build stage.
6. Rebuild the NGIAB Docker image.
7. Download the data package from Box if you haven't already.
8. Test your NGIAB build with this command:

  For Docker users:

   ```bash
   docker run --rm -it -v "/absolute/path/to/test/package:/ngen/ngen/data" name-of-your-NGIAB-image /ngen/ngen/data
   ```

  For Podman users:

   ```bash
   podman run --rm -it -v "/absolute/path/to/test/package:/ngen/ngen/data" name-of-your-NGIAB-image /ngen/ngen/data
   ```

   Follow the interactive terminal prompts to run NGIAB.

If your NGIAB run successfully executes, you've integrated the model!

For detailed instructions, follow the [detailed guide](i-do-not-know-docker.md).
