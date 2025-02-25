---
layout: default
title: Environment Management
parent: JupyterHub
nav_order: 6
has_children: false
description: ""
permalink: /jupyterhub/environment-management
---

# Environment Management
This documentation provides a comprehensive guide to managing environments using Conda and Mamba.
Provided Jupyter notebook images should have both Conda and Mamba pre-installed.
You will learn how to create, list, activate, and manage environments, as well as how to install and maintain packages within them. 

## Environment Management vs. Package Management
- **Environment Management:** Environment management allows users to create isolated environments that contain specific versions of languages like Python and packages, ensuring reproducibility and dependency control. Environments are usually created and mainained per project.
- **Package Management:** Package management is concerned with installation, updates, and removal of packages, ensuring software dependencies are met within a given environment.

## Understanding Conda and Mamba
Conda and Mamba are package and environment management tools primarily used in data science and software development. 
- **Conda** is an open-source package and environment manager that helps users install, run, and update packages and dependencies efficiently. Conda comes with the Anaconda distribution of Python.
- **Mamba** is a faster, drop-in replacement for Conda, designed to improve performance when solving package dependencies and installing software. We recommend using mamba over conda whenever possible.

### Mamba vs. Pip
While both Mamba and Pip are used for package management, they have key differences:
- **Mamba (or Conda) vs. Pip:**
  - **Mamba** is an environment manager that handles binary packages and dependencies using Conda environments. It ensures package compatibility across different platforms and Python versions.
  - **Pip** is the default Python package manager, primarily used to install Python packages from the Python Package Index (PyPI). Pip relies on virtual environments (e.g., `venv` or `virtualenv`) for isolation but does not manage dependencies as efficiently as Mamba.
  - **Dependency Resolution:** Mamba provides more robust dependency resolution compared to Pip, which may run into conflicts when installing packages.
  - **Speed:** Mamba is significantly faster than Pip when solving complex dependencies since it uses a more optimized dependency solver.

## Ensure the Base Environment is Activated
To start, activate the base environment:
```bash
source activate base
```
You should see the terminal add `(base)` in front of your terminal line, like so:
```bash
(base) jovyan@jupyter-maztec-40sdsu-2eedu:~$
```
**Note:** You may modify the base environment, but any changes will be reverted when your notebook server is restarted. If you need a persistent environment read the section [Creating a New Conda Environment](/jupyterhub/environment-management#creating-a-new-conda-environment).

## Searching for Packages on Anaconda.org
Anaconda.org is a repository where users can find and download Conda packages.
The `conda-forge` channel is a community-driven collection of packages that are regularly updated and maintained.
We recommend using the conda-forge channel whenever possible.

### Searching for Packages via Terminal
To search for packages on Anaconda.org via terminal, you can use the following command:
```bash
mamba search [package-name]
```
Example:
```bash
mamba search -c conda-forge matplotlib
```

### Using the Conda-Forge Channel
Many open-source packages are hosted on `conda-forge`, and you may need to specify this channel when installing a package:
```bash
mamba install -c conda-forge [package-name]
```
Example:
```bash
mamba install -c conda-forge r-base r-gdistance
```
You can also add `conda-forge` as a default channel to avoid specifying it every time:
```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
```
**Note:** Modifying the config for an environment **MUST** be done using `conda` over `mamba`.

## Creating a New Conda Environment
You may create persistent conda environments stored on your persistent storage.
To do so, you must use the `--prefix` option over specifying a name.
If you specify a name, it will be created under `/opt/conda` which will be reverted upon a notebook server restart.
You should keep in mind that additional conda environments will consume your persistent storage, so make sure to periodically check your [storage consumption](/jupyterhub/faqs/diskquota).
Conda environments **must not** be created on shared storage as this will negatively impact storage performance.
If you need to share an environment, you can [export an environment](/jupyterhub/environment-management#exporting-an-environment) to file and you may share the file via shared storage.

### Create an Empty Environment:
```bash
mamba create --prefix [file-path-here]
```
Example:
```bash
mamba create --prefix ~/my-env
```

### Create an Environment with One or More Packages:
```bash
mamba create --prefix [file-path-here] [package1] [package2] [package3]
```
Example:
```bash
mamba create --prefix ~/my-env r-base r-gdistance r-lme4
```

## Listing Your Environments
To list all your environments:
```bash
mamba env list
```
You should see your new environment along with the base environment:
```bash
/home/jovyan/my-env
base * /opt/conda
```

## Activating Your Desired Environment
To activate an environment:
```bash
conda activate [name or /path/to/environment]
```
Example:
```bash
conda activate /home/jovyan/my-env
```
**Note:** Activating an environment **MUST** be done using `conda` over `mamba`.

You should see your environment change in the terminal:
```bash
(/home/jovyan/my-env) jovyan@jupyter-maztec-40sdsu-2eedu:~$
```

## Installing or Building Additional Packages
Once the environment is activated, you can install additional packages:
```bash
mamba install -y [package1] [package2] [package3]
```
Example:
```bash
mamba install -y r-base r-gdistance
```

## Checking Installed Packages
To check what packages are installed in the activated environment:
```bash
mamba list
```

To check the version of a specific software package:
```bash
mamba show [package]
```
Example:
```bash
mamba show r-base
```

Now, you can run code from within this terminal session, and it will execute inside the activated environment.

**Note:** 
If you open a second terminal window, it will revert to the default state with no environment activated:
```bash
jovyan@jupyter-maztec-40sdsu-2eedu:~$
```

## Reusing an Existing Environment

### Open a New Terminal Window
Ensure the base environment is activated:
```bash
source activate base
```

### List Your Environments
```bash
mamba env list
```

### Activate Your Desired Environment
```bash
conda activate [name or /path/to/environment]
```
Example:
```bash
conda activate /home/jovyan/my-env
```
**Note:** Activating an environment **MUST** be done using `conda` over `mamba`.


You should see your environment change in the terminal:
```bash
(/home/jovyan/my-env) jovyan@jupyter-maztec-40sdsu-2eedu:~$
```

Now, you can run your code in your environment from the terminal.

## Exporting an Environment
Exporting an environment allows you to save its configuration in a YAML file. This is useful for sharing environments with others or reproducing the same setup on another machine.

### Exporting an Environment to a YAML File
To export the currently activated environment:
```bash
mamba env export > environment.yaml
```
This will create a file named `environment.yaml` that contains a list of all installed packages and their versions.

### Exporting an Environment Without Build Information
If you want a cleaner export without build-specific details, use:
```bash
mamba env export --no-builds > environment.yaml
```

### Recreating an Environment from a YAML File
To recreate an environment from an exported YAML file:
```bash
mamba env create -f environment.yaml
```

This will create a new environment with the same package versions as the exported one.

### Sharing an Environment
You can share the `environment.yaml` file with collaborators to ensure they use the same dependencies and configurations.
