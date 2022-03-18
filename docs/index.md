# GRASPED Software Systems

The offical technical documentation site for STAMINA4Space - GRASPED Software Systems. This guide contains information on using, deploying, and managing various softare systems developed by the STAMINA4Space Program.

### User Guides
These guides contain instructions and information on how to use the software.

* [Data Processing Tools](/user/data_processing_tools)
* [Diwata Browser for QGIS](/user/diwata_browser_qgis/)
* [Operations Software](/user/operations_software/) - Guides for regular users of the Operations Software
* [Operations Software](/user/operations_software/) - Guides for administrators. Find instructions on how to manage accounts and other admin options.
* [PR Website](/user/pr_site/) - Learn how to add and manage articles for [stamina4space.upd.edu.ph](https://stamina4space.upd.edu.ph/).

### Technical Guides
These guides contain instructions and information needed to maintain the software.

## Operations Software
The Diwata Operations Software (DOS) is an integrated system containing modules for
managing different components within the satellite operations workflow. The DOS is divided into
three major modules: Missions Management, Data Management, and Satellite Health
Management.

From an architectural perspective, the DOS serves as a central repository of data for multiple software systems developed in GRASPED.

Check out the [Operations Software User Guide](/user/operations_software) for more info.

## PR Website

## Diwata Browser QGIS Plugin
The Diwata Browser is a QGIS plugin that enables users to browse and load Diwata microsatellite images. It uses the STAMINA4Space operations-software backend to serve these images. The plugin utilizes the Spatiotemporal Asset Catalog (STAC) specification to describe the images information. The plugin can browse different collections from the operations-software catalog. Furthermore, the plugin can load selected items based on the loaded collection. The software is free to use.

The repository of this software can be found on: https://gitlab.com/cnpante/diwata-browser


## Data Processing Tools

The data-processing-tools toolkit is a collection of scripts to automate various tasks within the satellite data processing pipeline. This ranges from the pre-processing of raw files, up to publishing of processed Diwata data into the distribution site.


## Diwata Image Browser
The Diwata Image Browser is a public facing website developed during the PHL-Microsat program for distributing Diwata images. As of writing, it has served around 1200 users across different sectors.

## OpenDataCube
