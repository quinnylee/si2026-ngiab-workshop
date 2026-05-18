# Detailed guide

## Reminders

The workshop team has a dummy Python BMI model (regardless of input, every output value is 0) that they would like you to integrate into NGIAB. The model source code is located in a [GitHub repository](https://github.com/quinnylee/bmi_dummy). The model is also published on [PyPI](https://pypi.org/project/bmi-dummy/). An input data package for testing the dummy BMI model within NGIAB is stored in [Box](https://alabama.box.com/s/8sx72ozsg9uxh3e61how7fi0qoolpy1w).

The requirements for a Python model to be integrated into NGIAB are as follows:

- Must be compatible with Python 3.11
- If the following dependencies are included, they must have these specific versions:
  - `netCDF==1.6.3`
  - `pydantic<2`
  - `pandas<3`
- PyPI distributions available for all models and non-standard dependencies

## Instructions

1. Please make sure your computer meets the requirements in our ["Before the Workshop" guide](../before-the-workshop.md).
2. Check the model source code to make sure that it meets the requirements for integration into NGIAB. The model's `pyproject.toml` file (available on the GitHub repository) contains all the information needed to verify that the model meets the requirements listed above.
   1. `requires-python = ">=3.9"`, so it is compatible with Python 3.11
   2. `netCDF`, `pydantic`, and `pandas` are not listed as dependencies, so we have no known dependency conflicts.
   3. There is a PyPI distribution for the model linked above.
3. Navigate to the directory where you cloned the NGIAB source code. NGIAB is built with the Dockerfile located in `docker/Dockerfile`. Examine the Dockerfile's structure.
   1. Dockerfiles give instructions to a virtual machine in a specific order. This is helpful for building specific environments (Docker images) that need to be replicated. Dockerfile builds occur in "stages", which are marked by lines that look like `FROM rockylinux:9.1 AS base`. In this case, the `rockylinux:9.1` Docker image from Dockerhub is used and renamed to `BASE`. In the following lines, various packages are installed that are required to◊ build the rest of the NGIAB packages. The `base` stage lasts until you see another stage change at `FROM base AS build_base`.
   2. The `final` stage is the only stage that is accessible once Docker image is built. It is important that your model is accessible from this stage. Where does this stage begin? (Answer: the line with `FROM base AS final`)
4. Build the Docker image before you make any changes so you can watch how Dockerfiles build Docker images in real-time.
   1. Make sure your working directory is the `docker` directory within the `NGIAB-CloudInfra` directory. To check your working directory, use `pwd` (**P**rint **W**orking **D**irectory). You can use `cd path/to/desired/directory` to **C**hange **D**irectories to the `docker` directory.
   2. Use this command to build your Docker image.

   For Docker users:

    ```bash
    docker build -f Dockerfile -t devcon-workshop .
    ```

   For Podman users:

      For Docker users:

    ```bash
    podman build -f Dockerfile -t devcon-workshop .
    ```

    The `-f` tag indicates the location of the Dockerfile relative to the build context, the `-t` tag specifies the name of your new Docker image, and `.` is the build context (the current working directory).

5. Edit the Dockerfile to make sure that the bmi-dummy Python package is installed in the NGIAB image.
   1. Insert this line in the `final` stage after the dHBV install. Make sure to save!

    ```Dockerfile
    RUN uv pip install bmi-dummy
    ```

   A `RUN` command tells the virtual machine to run the following command. In this case, the `bmi-dummy` Python package distributed on PyPI is being `uv pip install`ed into the virtual machine. (`uv pip install` is the same as `pip install`, but faster.)

6. Rebuild the NGIAB Docker image. Use the same `docker build` command as before.
7. Download the data package from Box if you haven't already.
8. Run NGIAB with this command. Make sure to replace `/absolute/path/to/test/package` with the actual absolute path to the downloaded data package. Follow the interactive terminal prompts once the container (the virtual machine containing the image) is started.

   For Docker users:

   ```bash
   docker run --rm -it -v "/absolute/path/to/test/package:/ngen/ngen/data" devcon-workshop /ngen/ngen/data
   ```

   For Podman users:

   ```bash
   podman run --rm -it -v "/absolute/path/to/test/package:/ngen/ngen/data" devcon-workshop /ngen/ngen/data
   ```

   In this command, `--rm` tells Docker to automatically delete the container once the process is finished. `-it` tells Docker to open an interactive terminal. `-v` tells Docker to "mount" a local directory to a specific location in the virtual machine (like sticking a flash drive into a computer), and the following arguments take the form of `/path/to/local/directory:/path/to/virtual/directory`. `devcon-workshop` is the name of the image we are running (set in steps 3 and 5). `/ngen/ngen/data` runs the NGIAB guide script.

If your NGIAB run successfully executes, you've integrated the model!

## Troubleshooting

### Running out of memory during builds

Try changing every instance of `-j $(nproc)` to a small number of processes, like `-j 4` or `-j 2`.

### Podman NGIAB run randomly crashes

Your Podman virtual machine may default to a smaller size than is necessary for NGIAB. Try restarting the container and setting the amount of RAM to something more appropriate and then try running NGIAB again:

```bash
podman machine stop
podman machine init -m 8192 # 8 GiB of RAM
podman machine start
```
