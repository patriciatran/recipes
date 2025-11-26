<!--
   Copyright 2024, Center for High Throughput Computing, University of Wisconsin - Madison

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->

# [conda-env-yaml](/software/Conda/conda-env-yaml)

Container definition file for installing packages using conda and an `environment.yml` file.

# Overview

** Who would need this? ** Conda is a widely used package managers for both python-based and non-python packages, and contains software from many fields of study. Because of it's simplicity to manage software dependencies, many researchers might be familiar with installing conda packages on their laptop or a single server (e.g. a lab server). However, if you want to reproduce that environment in one of your HTCondor job, it's not as simple as typing `conda activate` in your executable file. How do you do this then?

** Overview ** The idea is to generate a `yml` file from your existing `conda environment`, transfer this file to the access point, and use it to build a container image file. The container image file can then be called in your HTCondor submit script, and activated it in your script.

>[!INFO] For more information about conda files and conda yml files, [visit the conda documentation](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html)

# Step by step:

This guide assumes that you already have a conda environment that you want to export. 
If you don't you can create one by typing this, from a Terminal window on your laptop:

```
# assumes conda is installed previously
conda create -n MyEnvironment
conda activate MyEnvironment
conda install -c channel package1 package2 package2
conda deactivate
```

Replace MyEnvironment with the environment name you want, and channel and packages with those you wish. 

## 1. The environment file

This recipe uses an `environment.yaml` file to provide the list of conda packages to install.
To create such a file, follow these steps:

1. Activate the conda environment you want to replicate.

```
conda activate MyEnvironment 
```
You will know that the environment is activate is your see it in parenthesis in front of your terminal prompt:
eg. `(MyEnvironment) name@laptop ~ % `

2. Export the conda environment using the following command:

   ```
   conda env export --from-history > environment.yaml
   ```

3. Copy the `environment.yaml` to the location where you will be running **your container build command**.
   **This recipe assumes that the `environment.yaml` file is in the same directory as the container build file!**
   If you are using Docker to build the container on your device, move the file to the same directory as your Dockerfile.
   
   If you are using Apptainer on HTC to build the container, transfer the file to login server, and then include the file in
   the `transfer_input_files` line of your `build.sub` submit file.

For more information on the `environment.yaml` file, see the [Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-an-environment-file-across-platforms).

>[!TIP] If you don't know what a built file is, visit this [documentation](https://chtc.cs.wisc.edu/uw-research-computing/apptainer-htc.html#start-an-interactive-build-job). If you don't know how to copy a local file to the server, visit [this page](https://chtc.cs.wisc.edu/uw-research-computing/transfer-files-computer)

 3.1 Docker
 Instructions TBD

 **3.2 Apptainer build**

Login to the server:
```
ssh netid@address
# enter password
# cd into the folder where your generic build file is
# for example if you have a folder called recipes where you usually store all your def files:
cd recipes
# obtain a copy of the conda-env-yaml.def template:
wget https://github.com/CHTC/recipes/raw/refs/heads/main/software/Conda/conda-env-yaml/conda-env-yaml.def
```

### Modifying the conda-env-yaml.def file

Edit the conda-env-yaml.def file to and replace `environment.yaml` with the file name of the yaml file YOU created. 
For example, environment.yaml --> MyEnvironment.yaml. 


## Naming the environment

By default the name of the environment that will be created will be the same as the environment you used to create the `environment.yaml` file.
If you want the environment name to be different, you can override it by replacing the line.

Additionally, another line you can modify in the `conda-env-yaml.def` file is how you want the environment to be named. 

Currently, the template shows:
```
conda env create -f /environment.yaml
```

You can add -n `your_environment_name` to give it another name

```
conda env create -n your_environment_name -f /environment.yaml
```

Once your conda-env-yaml.def file has been edited, save.

Next  modify your generic `build.sub` file and ensure the `transfer_input_files==` line mentions the files needed to build the container:
- environment.yaml (or whatever name file you used)
- conda-env-yaml.def

for example: `transfer_input_files == environment.yaml, conda-env-yaml.def`

Interactively start a job to build your container:
```
# submit interactive job
condor_submit -i build.sub
# check where you are
pwd
# build the container
apptainer build yourEnvironmentName.sif conda-env-yaml.def
# If no error message, proceed with testing:
apptainer shell -e yourEnvironmentName.sif
# type some commands from packages you expect to be in there such as manuals, -h pages.
# exit the container
exit
# move the container image to your staging folder
mv yourEnvironmentName.sif /staging/netid/.
```
You can now your the container image (`.sif`) file in your submit file by specifying `container_image=file:///staging/netid/yourEnvironmentName.sif`


## Using the environment in jobs

When using this container, run the following commands in order to activate the environment:

```
source activate
conda activate your_environment_name
```

For HTC jobs, you should add these lines to the top of your executable `.sh` file.

## [conda-env-yaml.def](conda-env-yaml.def)

| | | |
| ---: | :--- | :--- |
| *Type* | **Apptainer** | |
| *OS* | Debian "bullseye" | |
| *Base image* | **continuumio/miniconda3:latest** | *DockerHub* |
| *Updated* | 2024-05-09 | *Andrew Owen* |
| *Last tested on HTC* | 2024-05-09 | *Andrew Owen* |
| *Last tested on HPC* | - | - |

## [Dockerfile](Dockerfile)

| | | |
| ---: | :--- | :--- |
| *Type* | **Apptainer** | |
| *OS* | Debian "bullseye" | |
| *Base image* | **continuumio/miniconda3:latest** | *DockerHub* |
| *Updated* | 2024-05-09 | *Andrew Owen* |
| *Last tested on HTC* | 2024-05-09 | *Andrew Owen* |
| *Last tested on HPC* | - | - |

