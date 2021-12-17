---
title: Data Processing Tools User Guide
---

# Data Processing Tools User Guide

## Installation

### Requirements

The following are required in order to use the Data Processing Tools software.

1. **Git** - Git controls and manages software repositories. Users can download and install Git depending on their operating system. Users can download Git on this link: https://git-scm.com/downloads

2. **Gitlab Account** - Gitlab stores and monitors changes in DPT software. A Gitlab account linked to GRASPED group is needed to view the DPT repository. The account must use an SSH key on their machine. Use this instruction to set up SSH key: https://docs.gitlab.com/ee/ssh/.

3. **Anaconda** - Anaconda is the distribution of the Python packages required by the DPT software. Use this instruction to install Anaconda dependending on the platform: https://docs.anaconda.com/anaconda/install/windows/

4. **Rclone** - Rclone is a command line program to manage files on cloud storage. This program is needed in order to synchronize products to your local machine. Use this instruction to download Rclone: https://rclone.org/downloads/


### Configuring Rclone

To configure Rclone with the S4S Google Drive, follow these steps:

1. It is recommended to copy the Rclone download to your GRASPED folder.

        (base) C:\User\Documents\GRASPED\rclone-v1.54.0-windows-amd64 >

2. From your rclone directory, type in the following:

        > rclone config

3. Type in the following to create a new remote.

        e/n/d/r/c/s/q> n

4. Type a name for the remote, in this case, it is recommended to name it with:

        name> s4s-drive

5. Type in the following to select Google Drive:

        Storage> drive

6. Press enter to set default for client_id.

        client_id>

7. Press enter to set default for client_secret.

        client_secret>

8. Type in the following to set the scope of your access.

        scope> 1

9. Press enter to set default for client_secret.

        root_folder_id>

10. Press enter to set default for service_account_file.
    
        service_account_file>

11. Type in the following to select say no to editing advanced config:

        y/n> n

12. Type in the following to select say yes to use auto config:

        y/n> y

13. This will open a browser in which you are asked to login your google account. Choose your S4S google account.

14. If needed, enter your password.

15. Click “Allow” to allow access of rclone to your drive as well as the S4S drive.

16. Return to the Anaconda prompt.

17. Type in the following to say no to set the config as team drive:

        y/n> n

18. Press enter or say yes to set the config.

        y/e/d> y

19. Finally, enter the following to exit the rclone config.

        e/n/d/r/c/s/q> q

20. You must be returned to your rclone directory.

### Data Processing Tools Installation
The following installs Data Processing Tools on your machine.

#### On Windows

1. Open Anaconda Prompt.
2. Go to the directory where you want to save the DPT software. Ex.

        (base) C:\User\Documents\GRASPED >

3. Run each line of code:
        cd Documents\GRASPED
        git clone git@gitlab.com:grasped/data-processing-tools.git
        cd data-processing-tools
        conda create -n data-processing-tools -c conda-forge python=3.7 gdal rasterio geopandas fiona opencv cartopy arosics pyqt
        conda activate data-processing-tools
        pip install -r requirements.txt --user

#### On Debian

1. Open a terminal.
2. Go to the directory where you want to save the DPT software. Ex.

        (base) /home/User/Documents/GRASPED $


3. Run each line:

        cd Documents/GRASPED
        git clone git@gitlab.com:grasped/data-processing-tools.git
        cd data-processing-tools
        conda create -n data-processing-tools python=3.7
        conda activate data-processing-tools
        conda install -c menpo opencv
        conda install -c conda-forge gdal=2.4.4 rasterio pyproj pyqt
        pip install -r requirements.txt

## Graphical User Interface (GUI)

## Updating Data Processing Tools
There can be new features that require new dependencies. In this case, the software needs to be updated and reinstalled. Follow these steps to update and reinstall DPT. Refer to the Software Systems team for new features.
