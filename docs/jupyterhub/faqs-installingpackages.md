---
layout: default
title: Installing Packages
parent: Frequently Asked Questions
grand_parent: JupyterHub
nav_order: 7
description: ""
permalink: /jupyterhub/faqs/installingpackages
---

# Installing Software Packages
In addition to the pre-installed software packages detailed in [Available Images](/jupyterhub/images), users can install additional software to their environment either directly within the Jupyter Notebook or from the integrated terminal.

### Method 1: Installing Packages Directly in a Jupyter Notebook
You can install Python packages directly from a Jupyter Notebook using the `!` command to run shell commands. Follow these steps:

1. Open your Jupyter Notebook

1. Create a new code cell

1. Use the following command to install a package (replace `package_name` with the name of the package you want to install):
```bash
!pip install package_name
```

1. Run the cell by pressing `Ctrl + Enter`. The package will be installed, and you’ll see the installation progress in the output

1. Import the package to use it in your notebook:
```python
import numpy as np
```

### Method 2: Installing Packages via the Terminal
If you prefer using the terminal, follow these steps:

1. Launch a Terminal:
- From the dashboard, click on the big blue "+" in the top left corner
- Select "Terminal" from the Launcher menu
- Install the package using pip in the terminal. For example, to install pandas, type:
```bash
pip install pandas
```

1. Press `Enter` to execute the command. You’ll see the installation progress in the terminal

1. Return to your notebook to import and use the package:
```python
import pandas as pd
```

### Method 3: Installing Packages Using Anaconda
If you're more familiar with using Anaconda as your package manager, follow these steps:

1. Go to [Anaconda.org](https://anaconda.org/){:target="_blank"}
1. Search for packages:
   - In the search bar, type the name of the package you are looking for (e.g., `numpy`)
   - Press `Enter` to view the search results
   - On the search results page, you can filter the results by the `conda-forge` channel
1. Select a package:
    - Click on the package name to view more details, including installation instructions and available versions
1. Open your Jupyter Notebook
1. Launch a Terminal:
    - From the dashboard, click on the big blue "+" in the top left corner
    - Select "Terminal" from the Launcher menu
1. Install **Mamba** (faster than Conda):
    - If you haven't already installed `mamba`, you can do so in the terminal with:
    ```bash
    conda install mamba -n base -c conda-forge
    ```
1. Install the desired package with Mamba:
    ```bash
    mamba install package_name -c conda-forge
    ```
1. Return to your notebook to import and use the package:
    ```python
    import package_name
    ```