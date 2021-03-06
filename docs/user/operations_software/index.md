---
title: Operations Software User Guide
---

# Diwata Operations Software User Guide

The Diwata Operations Software (DOS) is an integrated system containing modules for
managing different components within the satellite operations workflow. The DOS is divided into
three major modules: Missions Management, Data Management, and Satellite Health
Management.

From an architectural standpoint, the DOS serves as a central repository of data for multiple software systems developed in GRASPED.

![Overview Diagram of GRASPED Operations Software](images/operations-software-overview.png)

## Accessing the Website

The Diwata Operations Software may only be accessed by authorized users using this [link](https://ops.stamina4space.upd.edu.ph/).
![Screenshot of the homepage](images/operations-software-homepage.png)

## Missions Management

The Missions Management module was designed to help in mission planning and management of satellite passes. This module includes automated pass prediction features, similar to the Microsatellite Scheduler, as well as a satellite command generator.

### Targets

![Overview of Targets Page](images/targets-overview.PNG)

<center>Overview of Targets Page</center>

Page contents:

1. Create new targets
2. Filters
3. Actions (Delete multiple targets)
4. Search bar to find keywords on the table
5. View/edit target details

Filter Description

| Filter Label   | Description              | List of Values                                                                                                                                                                                                                                                                                                                      |
| -------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name           | Filter by name           | ----                                                                                                                                                                                                                                                                                                                                |
| Classification | Filter by classification | Integer from 0-5 </br> Default value: All                                                                                                                                                                                                                                                                                           |
| Purpose        | Filter by purpose        | Agriculture, Agriculture Validation, Agroforestry, Aquaculture, Calibration Validation, Coastal Monitoring, Cultural Heritage, Fisheries, Forest Validation, Forestry, Flood and storm surge, Land Use Change, Ocean Validation, Protected Area, Security, Typhoon, Tourism, Urban, Validation Site(urban) </br> Default value: All |

#### How to create new entries?

1. Click the `Add Target` button at the upper right side of filters to redirect to Add Target Page.

<figure markdown>
   ![Add Target Page](images/add-target-page.PNG)
   <figcaption>Add Target Page</figcaption>
</figure>

<br />&nbsp;&nbsp;&nbsp;&nbsp; 2. Input name, select target by drawing polygon on the map, tick/untick is active status, select classification and purpose, and input requred bands.
<br />&nbsp;&nbsp;&nbsp;&nbsp; 3. Name, classification and purpose are required fields.
<br />&nbsp;&nbsp;&nbsp;&nbsp; 4. After filling out the form, click `Submit` button.
<br />&nbsp;&nbsp;&nbsp;&nbsp; 5. New target will be added to the table in Targets Page.

#### How to view or update details of a mission?

1. Click the ID of target to redirect to View Target Page.
2. If details are edited, click `Submit` button to update.

### Acquisition Missions

![Acquisition Missions](images/missions-table.PNG)

Basic functions on Acquisition Mission includes, add mission, filter missions, edit commands on the table and perform batch actions.

1. Click the Add Mission. Add the necessary parameters for the mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
2. Perform filters by selecting values on the drop-downs.
3. Type a word on search bar to find keywords on the table.
4. You can perform batch actions when selecting an item on table. You can select multiple. Then select an action from the drop down list. Click Go button to update selected items.

### Upload Missions

![Upload Missions](images/upload-missions.png)

Basic functions on Upload Mission includes, add upload mission, filter missions, edit commands on the table and perform batch actions.

1. Click the Add Upload Mission. Add the necessary parameters for the upload mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
2. Perform filters by selection values on the drop-downs.
3. You can perform batch actions when selecting an item on table. You can select multiple.

![Batch](images/upload-mission-batch.png)

### Download Missions

![Batch](images/download-mission.png)

Basic functions on Download includes, add download mission, filter missions, edit commands on the table and perform batch actions. Fairly similar with the Upload mission module.

1. Click the Add Download Mission. Add the necessary parameters for the download mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data. Some settings are Diwata-2 specific settings so you need to input necessary parameters for the compiler to work properly. Add the bands, starting addresses, and number of images to be downloaded, respectively.

2. Perform filters by selection values on the drop-downs.
3. You can perform batch actions when selecting an item on table. You can select multiple.

### ACU Log Download Missions

![Batch](images/acu-log.png)

1. You may click the ACU Log Download Missions button to view and update the current records or the Add button if you want to add a new record.

![Batch](images/add-acu-log.png)

Add the necessary parameters for the mission, and then click Save.

2. Perform filters by selecting values on the drop-downs.
3. You can view details of the ACU log item when you click the ID. You can also perform batch operations by checking the check-boxes. You can select multiple.
4. You can also view the commands by click the items on the Command column.

### Commands and Command Batches

#### Command Module

![Command](images/view-command-list.png)

Commands are generated by default whenever missions are created. The list of commands can be found in the Command tab.

#### Viewing a command

- Upon clicking the Commands button, you will see all the generated commands from all the different missions (acquisition, download, etc.). Click on the ID to view/edit the content of a command.

![Command Batches](images/view-command.png)

You can now edit the fields and click save after performing a change.

<b>Note:</b> Commands that are created using the Add command module cannot be linked to an existing mission. To be able to link a mission and a command, create a mission and then find the generated command and edit it.

<hr/>

#### Command Batch Module

![Command Batches](images/command-batches.png)

Command batches are a group of commands. It may be a combination of upload, download, acu log download, and acquisition missions.

![Command Batch Add](images/add-command-batch.png)

1. To create a batch, go to the command batches module or click the Add button.
2. Add the necessary parameters.

#### Command Batch List

![Command Batch list module](images/comand-batch-list-add.png)

Supply the ID of the command that will be added to the batch. You can add multiple.

### Reports

![Overview of Reports Page](images/reports-overview.PNG)

<center>Overview of Reports Page</center>

Page contents:

1. Date Filter
2. Satellite Filter
3. Payload Filter

#### How to apply filters?

1. On the upper left side, select a satellite to filter data shown on graphs and table. Once satellite filter is changed, graphs and table will reload and new data will be fetched based on the selected satellite. Default value of satellite filter is Diwata-1.
2. On the upper right side is the date range filter. To apply this filter, select two dates from the date range selector. To cancel this filter, click the `All` button. As default, data shown on graphs and table shows `All` or no date range is initially set.
3. From the PERCENTAGE COVERED IN THE PHILIPPINES, data shown on the map can be filtered by payload. Initially, data captured by SMI, HPT and ERC are included. Toggle the payload to exclude it on map.
4. Refresh page to reset all filters.

## Data Management

The Data Management Module contains tools for storing and accessing images and metadata in different levels of data processing. In this module, users may download images for processing and upload derived products.

A Data Processing Pipeline is also integrated into the module to automate most of the existing Diwata-1 and Diwata-2 Level 1 product generation.

### Raw Files

![Overview of Raw Files Page](images/raw-files-overview.PNG)

<center>Overview of Raw Files Page</center>

Page contents:

1. Filters
2. Download options
3. Search bar to find keywords on the table
4. View more details

<!-- #### How to apply filters to the table? -->

Filter Description

| Filter Label      | Description                                  | List of Values                                                                         |
| ----------------- | -------------------------------------------- | -------------------------------------------------------------------------------------- |
| Type              | Filter by type                               | XTLM, QTLM                                                                             |
| Receiving Station | Filter by receiving station                  | ASTI, TU, Hakodate, Kiruna                                                             |
| Packet Loss       | Filter by range of packet loss in percentage | Accepted Values: Integer from 0 to 100                                                 |
| Satellite         | Filter by satellite                          | Diwata-1, Diwata-2, Maya-1                                                             |
| SHU Address       | Filter by SHU Address                        | Accepted Value: Positive Integer                                                       |
| Upload Time       | Filter by range of upload time.              | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Created Time      | Filter by range of created time.             | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Capture Time      | Filter by range of capture time.             | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Download Time     | Filter by range of download time.            | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Uploader          | Filter by uploader                           | -----                                                                                  |

<br />

#### How to view more details?

1. From the selected row, click the Eye Icon at the last row to display the drawer containing details of the selected capture file.
2. The drawer has the following details:
   <br />&nbsp;&nbsp;&nbsp;&nbsp; a. Image - click the icon at the top right side of the image to view in full screen. Press Esc to exit full screen. Available in Raw Image Page ang Merged Raw Image Page.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; b. Capture details
   <br />&nbsp;&nbsp;&nbsp;&nbsp; c. Files - list of all downloadable files and images. Click the link to download.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; d. Extracted Images - Click link to view image in new tab. Available in Telemetry Page only.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; e. Raw Images - Click link under Raw column to download the `.rg3` file. Click `.bin` link under Telemetry column to redirect to Telemetry, a drawer will appear to show details of Telemetry. Available in Merged Raw Images Page only.

#### How to download data?

Option 1: From table, using checkbox

1. Tick the row/s you plan to download.
2. Download button will be enabled when at least one row is selected.
3. Choose from the file formats option you wish to download.
4. Once a file format is selected, download will automatically proceed.

Option 2: From Drawer

1. From the chosen row, click the Eye Icon at the last row to display the drawer containing details of the selected capture file.
2. Look for the `Files` section on the drawer.
3. Click the link beside the file name you want to download.
4. If clicked link is an image (.jpg), new tab will be opened to view the image. For other file format, clicked link will automatically download the selected file.

### Captures

![Overview of Captures Page](images/captures-overview.PNG)

<center>Overview of Captures Page</center>

Page contents:

1. Filters
2. Actions for multiple selected rows:
   <br />&nbsp;&nbsp;&nbsp;&nbsp; a. Move to Raw - available in Level 0 Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; b. Move to Level 0 - available in Level 1 Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; c. Add to Cart - for batch download, not available in Level 1 Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; d. Download options
   <br />&nbsp;&nbsp;&nbsp;&nbsp; e. Sync to Distribution - available in Level 1 Page
3. Capture tags (editable field)
4. More actions:
   <br />&nbsp;&nbsp;&nbsp;&nbsp; a. View more details
   <br />&nbsp;&nbsp;&nbsp;&nbsp; b. Move to Raw - available in Level 0 Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; c. Move to Level 0 - available in Level 1 Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; d. Add to Cart - for batch download, not available in Level 1 Page
5. View Cart

<br />
<!-- #### How to apply filters to the table? -->

Filter Description

| Filter Label | Description                                   | List of Values                                                                         |
| ------------ | --------------------------------------------- | -------------------------------------------------------------------------------------- |
| Capture ID   | Filter by Capture ID                          | ----                                                                                   |
| Is Published | Filter by publish status. <br /> Default: All | Published, Unpublished                                                                 |
| Band         | Filter by band. <br /> Multiple Select        | HPT-R, HPT-G, HPT-B, HPT-N, SMI-N, SMI-V, MFC, WFC                                     |
| Mission ID   | Filter by mission ID                          | ----                                                                                   |
| Created Time | Filter by range of created time.              | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Capture Date | Filter by range of capture time.              | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Packet Loss  | Filter by range of packet loss in percentage  | Accepted Values: Integer from 0 to 100                                                 |
| Products     | Filter by products. <br /> Multiple Select    | L1A, L1B, L1C, no L1A, no L1B, no L1C                                                  |
| Tags         | Filter by tags. <br /> Multiple Select        | Urban, Vegetation, Coastal, Agriculture, Forestry, Processable                         |

#### How to view more details?

1. From the selected row, click the Eye Icon at the last row to display the drawer containing details of the selected capture file.
2. The drawer has the following details:
   <br />&nbsp;&nbsp;&nbsp;&nbsp; a. Image - click the icon at the top right side of the image to view in full screen. Press Esc to exit full screen.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; b. Capture details
   <br />&nbsp;&nbsp;&nbsp;&nbsp; c. Files - list of all downloadable files and images. Click the link to download.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; d. Georeferencing (POC only)
3. Details from mission column is clickable to redirect to View Acquisition Mission Page.

#### How to download data?

Option 1: From table, using checkbox

1. Tick the row/s you plan to download.
2. Download button will be enabled when at least one row is selected.
3. Choose from the file formats option you wish to download.
4. Once a file format is selected, download will automatically proceed.

Option 2: From Drawer

1. From the chosen row, click the Eye Icon at the last row to display the drawer containing details of the selected capture file.
2. Look for the `Files` section on the drawer.
3. Click the link beside the file name you want to download.
4. If clicked link is an image (.jpg), new tab will be opened to view the image. For other file format, clicked link will automatically download the selected file.

Option 3: From download cart

1. From the chosen row, click the Cart Icon at the last row to add to cart.
2. `View Cart` button, beside Profile in navigation bar, will appear if at least one capture is added to cart.
3. Click `View Cart` button to redirect to Download Cart page.
4. You may choose to download all, delete all or delete individual capture from the list.

### Capture Groups

![Overview of Capture Groups Page](images/capture-groups-overview.PNG)

<center>Overview of Capture Groups Page</center>

Page contents:

1. Filters
2. Actions
3. Search bar to find keywords on the table
4. Capture ID - clickable to view and edit details
5. Is Published - Toggle to allow capture to be available in distribution site (Image Browser)
6. Capture tags (editable field)
7. Capture Group - clickable to redirect to View Capture Group Page, available in Capture Group Products Page
8. View details

<br />

<!-- #### How to apply filters to the table? -->

Filter Description

| Filter Label     | Description                                   | List of Values                                                                         |
| ---------------- | --------------------------------------------- | -------------------------------------------------------------------------------------- |
| Group Product ID | Filter by Group Product ID                    | ----                                                                                   |
| Is Published     | Filter by publish status. <br /> Default: All | Published, Unpublished                                                                 |
| Payload          | Filter by payload. <br /> Multiple Select     | HPT, MFC, WFC, SMI, ERC                                                                |
| Product Type     | Filter by product type.                       | Stacked, Mosaic                                                                        |
| Tags             | Filter by tags. <br /> Multiple Select        | Urban, Vegetation, Coastal, Agriculture, Forestry, Processable                         |
| Group ID         | Filter by Group ID                            | ----                                                                                   |
| Created Time     | Filter by range of created time.              | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |
| Upload Time      | Filter by range of upload time.               | Sample: 2022-01-12 00:00:00 ~ 2022-02-16 23:59:59 <br /> Format: `YYYY-MM-DD HH:MM:SS` |

#### How to view more details?

1. From the selected row, click the Eye Icon at the last row to display the drawer containing details of the selected capture file.
2. The drawer has the following details:
   <br />&nbsp;&nbsp;&nbsp;&nbsp; a. Image - click the icon at the top right side of the image to view in full screen. Press Esc to exit full screen.
   <br />&nbsp;&nbsp;&nbsp;&nbsp; b. Capture details
   <br />&nbsp;&nbsp;&nbsp;&nbsp; c. Capture Group Products - available in Capture Groups Page
   <br />&nbsp;&nbsp;&nbsp;&nbsp; d. Captures - available in Capture Groups Page
3. Another option is to click the ID from the `Id` column to redirect to View Page. From this page, some fields can be updated. Click `Save` button to submit changes.
4. On Capture Group Products, `Capture Group` column details can be clickable. Once cliked, you will be redirected to View Capture Group Page.

#### How to update publish status?

Option 1: From table, using checkbox

1. Choose an action from the dropdown list above the table.
2. Select caputure/s to update by ticking the check box on each row.
3. If at least one mission is selected, the `Go` button beside the action dropdown list will be clickable, and number of selected missions will be shown beside the Go button.
4. Click `Go` button to update selected missions.

Option 2: From table, toggle publish/unpublish

1. Toggle publish/unpublish under `Published?` column on the table.

Option 3: From View Capture Group Page

1. Click the capture group ID from the `Id` column to redirect to View Page.
2. Check/uncheck the publish status field or change other editable fields.
3. Click `Save` button to submit changes.

## Satellite Health Management

### ACU Logs

Attitude Control Unit (ACU) log files contain information recorded onboard the satellite describing the state of its positioning and orientation at a given time. The ground station operators download the log files regularly from the satellites and upload them into a cloud service.

Originally, the contents of the files can be inspected by manually running a depacketer software. This process was tedious and only provided information on-demand.

## Links

- [Operations Software](https://ops.stamina4space.upd.edu.ph/)
- [Operations Software Admin Site](https://api.ops.stamina4space.upd.edu.ph/admin/)
- [Operations Software on Gitlab](https://gitlab.com/grasped/operations-software/)
