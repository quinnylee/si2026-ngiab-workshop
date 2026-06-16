# Three ways to run NextGen in a Box (NGIAB)

Before starting this section, make sure you have `uv` installed, and make sure you have cloned the NGIAB-CloudInfra and DataStreamCLI repositories. See our [Before the Workshop](before-the-workshop.md) guide for instructions.

In this section, we will be running NOM+CFE+t-route for the area upstream of USGS gage 02342500 (Uchee Creek Near Fort Mitchell, Al.) over the time period 2020-01-01 to 2020-01-07.

## Data Preprocessor

The NGIAB Data Preprocessor has a built-in function to run NGIAB. This is convenient, but does not allow you to modify any of the preprocessed data files before the run starts. *Please note: this exercise is currently not supported for Podman.*

```bash
uvx -p 3.10 --from ngiab-prep cli -i gage-02342500 -sfr --start 2020-01-01 --end 2020-01-07 --source aorc --run
```

![image](demo_tapes/data_prep_ngiab-ezgif.com-cut.gif)

In this command, `uvx` allows for you to run the data preprocessor without installing it as a package. `-i gage-02342500` selects USGS gage 02342500 as the location to be modeled. `-s` **s**ubsets the NextGen hydrofabric geopackage, `-f` generates **f**orcings, `-r` generates **r**ealization and BMI configuration files, `--start` and `--end` specify the start and end dates of the simulation, `--source` specifies the source of raw forcing data, and `--run` indicates that a Docker image containing NGIAB should be spun up after the data has been preprocessed, and a NextGen simulation should be automatically run with the data.

For more information on the data preprocessor and its functionalities, see the [documentation](https://github.com/CIROH-UA/NGIAB_data_preprocess).

## Data Preprocessor + Guide Script

This method allows you to preprocess data and run NGIAB as separate steps. The guide script in the NGIAB-CloudInfra repository walks you through the NGIAB run and gives you the option to evaluate your results using TEEHR and visualize the results using the Tethys visualizer.

```bash
cd /path/to/NGIAB-CloudInfra
uvx --from ngiab-prep cli -i gage-02342500 -sfr --start 2020-01-01 --end 2020-01-07 --source aorc
chmod +x guide.sh
./guide.sh
# Podman users: ./guide.sh -p
```

![image](demo_tapes/data_prep_guide.gif)

![image](demo_tapes/vis.gif)

## CCNH in JupyterHub

Please follow the instructions in the [HydroShare resource](https://www.hydroshare.org/resource/27045581bdea4808a393330f2417379c/). Running NGIAB in CCNH allows you to bypass any setup steps and run the simulation in the cloud.

On the Hydroshare page, click on the "Open with..." dropdown menu and select "CIROH-2i2c JupyterHub (Workshop).

![image](demo_tapes/hydroshare.png)

The username and password will be shared during the workshop.
Select a medium "CIROH Community NextGen Hub" image.

![image](demo_tapes/2i2c.png)

Once your server starts, you can follow the Jupyter notebooks in this order:

1. NextGen Data Preparation
2. NextGen Run
3. NextGen TEEHR Evaluation
4. NextGen Output Analysis
5. NextGen Calibration

![image](demo_tapes/ccnh.png)
