# GRASPED Software Systems

The offical technical documentation site for STAMINA4Space - GRASPED Software Systems.

* Data Processing Tools
* Operations Software
* PR Website

## Operations Software
The Diwata Operations Software (DOS) is an integrated system containing modules for
managing different components within the satellite operations workflow. The DOS is divided into
three major modules: Missions Management, Data Management, and Satellite Health
Management.

In an architectural standpoint, the DOS serves as a central repository of data for multiple software systems developed in GRASPED.

See the [Operations Software User Guide](/user/operations_software) for more info.

## PR Website
The GRASPED project maintained two website for public relations and information dissemination: (1) The [PHL-Microsat Website](https://phl-microsat.upd.edu.ph) and, (2) its successor [the STAMINA4Space Website](https://stamina4space.upd.edu.ph). The purpose of both website was to make information about the projects accessible to the public.

The two websites were built on different platforms. See the [PR Website Technical Guide](#) for more information.


## Diwata Browser QGIS Plugin
The Diwata Browser is a QGIS plugin that enables users to browse and load Diwata microsatellite images. It uses the STAMINA4Space operations-software backend to serve these images. The plugin uses the Spatiotemporal Asset Catalog (STAC) specification to describe the images information. The plugin can browse different collections from the operations-software catalog. Furthermore, the plugin can load selected items based on the loaded collection. The software is free to use.

The repository of this software can be found on: https://gitlab.com/cnpante/diwata-browser


## Data Processing Tools

The data-processing-tools toolkit is a collection of scripts to automate various tasks within the satellite data processing pipeline. This ranges from the pre-processing of raw files, up to publishing of processed Diwata data into the distribution site.


## Diwata Image Browser
The Diwata Image Browser is a public facing website developed during the PHL-Microsat program for distributing Diwata images. As of writing, it has served around 1200 users across different sectors.

## OpenDataCube
