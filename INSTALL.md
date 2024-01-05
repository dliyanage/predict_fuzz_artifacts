# Installation

We offer a straightforward and convenient method to execute the `analysis.ipynb` notebook located in the `./scripts` directory.

## Prerequisites

Ensure that the following software is installed on your local machine (works for any operating system, including Windows, Mac, or Linux):
- The latest version of the statistical package *R*. If R is not available on your local machine, download R from [https://cran.r-project.org/bin/windows/base/](https://cran.r-project.org/bin/windows/base/) and follow the instructions provided in [this link](https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu) based on your operating system to install R.
- The individual edition of `Anaconda Navigator`. Find the installation package and instructions at [https://docs.anaconda.com/anaconda/navigator/](https://docs.anaconda.com/anaconda/navigator/)

## Clone and Launch JupyterLab

- Download this repository to your local machine.

We intend to launch the Jupyter workbooks using JupyterLab through Anaconda Navigator. Before launching, we need to create a new R environment within Anaconda Navigator, as our language of interest is R. Below are the steps to create a new R environment through the GUI.

- Open Anaconda Navigator.
- Navigate to the `Environments` tab from the menu on the left-hand side.
- Create a new R environment with the desired name by clicking the `Create` button. (Make sure you uncheck Python from the dialog box)
- Return to `Home`. Select the created R environment from the drop-down menu under `Applications on`.

Now you are all set for launching `JupyterLab`.

- Click on the JupyterLab `Launch` button from the home window of Anaconda Navigator.

JupyterLab will open in the browser.

## Open and Run Our Workbooks

The local file system appears in the left sidebar in the opened JupyterLab window. (If not, go to `View -> Show Left Sidebar` from the menu bar)

- Navigate to the artifacts repository in the local file system and open the `analysis.ipynb` notebook on JupyterLab.
- Before running the notebooks, make sure you have installed all the required R libraries mentioned in `REQUIREMENTS.md` and `README.md`. You can use the following line of code to install a required package:
  
```
install.packages(<package_name>, dependencies=TRUE)
# Example: If you need to install the package "ggplot2"
install.packages("ggplot2", dependencies=TRUE)
```

- If you need to install a specific version of an R package, you can use the following functions to achieve that. The specific R package versions are also stated in `REQUIREMENTS.md` and `README.md`. Let's take the same example *ggplot2*:

```
install.packages("remotes")
library(remotes)

install_version("ggplot2", version = "3.3.5", repos = "http://cran.us.r-project.org")
```

Now, you can edit and run the notebooks in this repository. Instructions on using JupyterLab can be found [here](https://jupyter.org/).

## Other Methods

We want to emphasize that this is not the only way to edit and run the Jupyter workbooks. However, we found this method very convenient to set up and execute. You are free to use any other method that serves the same purpose.
