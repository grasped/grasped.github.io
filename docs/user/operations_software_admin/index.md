---
title: Operations Software Admin Guide
---

# Diwata Operations Software Admin Guide

This page contains information on the Operations Software's admin page and other tools to manage data stored in the software.

Users can login directly to the operations software backend to have additional access to the database especially when correcting data that are not editable in the Diwata Operations Software. For security purposes, however, not all users who have access to the Diwata Operations Software can login to the admin site. Some users may have limited access as well to protect data integrity. Users can access the site using this [link](https://api.ops.stamina4space.upd.edu.ph/admin/).
![Screenshot of the homepage](images/operations-software-admin.png)

Upon logging in, the user will see all the accessible modules and data for viewing, adding, editing, and deleting. Access to these data and modules depends on the permissions granted to the user.
![Screenshot of admin homepage](images/admin-homepage.png)
![Screenshot of admin homepage-2](images/admin-homepage-2.png)
![Screenshot of admin homepage-3](images/admin-homepage-3.png)

## Users Module

### Adding user accounts

1. Under the **Authentication and Authorization** group, select **Users** or click the **Add** button.
![Screenshot of the site administration](images/add-users-1.png)
<br/><br/>Alternatively, you may also click the **Add User** button from the Users list.
![Screenshot of users list](images/add-users-2.png)

2. Add the desired username and password. **Note:** Avoid using common passwords.
![Screenshot of add users 1](images/add-users-3.png)

3. Click **Save** button. A message will confirm that the user has been saved and a dialog will open where you can edit user info and permissions.
![Screenshot of add users 2](images/add-users-4.png)

You may select individual/multiple permissions for each user.
![Screenshot of add users 3](images/add-users-5.png)

![Screenshot of add users 3](images/add-users-6.png)


## Missions Management

### Acquisition Missions

#### Adding a new Acquisition Mission

1. Login to the admin site with your credentials. You may contact the system administrator if you still don't have an account.

2. Look for the Missions module under the MISSIONS_MANAGEMENT subgroup.
![Screenshot of Acquisition Missions](images/acquisition-missions.png)

3. You may click the Missions button to view and update the current records or the Add button if you want to add a new record.
![Screenshot of Add Acquisition Missions](images/acquisition-missions-2.png)

4. Add the necessary parameters for the mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
![Screenshot of Add Acquisition Missions](images/add-mission-1.png)

Some settings are Diwata-2 specific settings so you need to input necessary parameters for the compiler to work properly.

![Screenshot of Add Acquisition Missions](images/add-mission-2.png)

Unchecking this box will prevent the command compiler from executing.
![Screenshot of Add Acquisition Missions](images/add-mission-3.png)

Add the payloads to be used.
![Screenshot of Add Acquisition Missions](images/add-mission-4.png)

**Note:** As of April 5, 2020, generating commands for Diwata-1 will result in TLE errors because Diwata-1 has already been decommissioned. Command Compiler tries to fetch the latest TLE values from the server and will return an error if the TLE's epoch date is older than 7 days.

#### Batch operations for Acquisition Missions

1. Click on the checkbox beside each record to select a record. You may also click the uppermost checkbox to select all the records in the current page.
![Screenshot of Batch Acquisition Missions](images/acquisition-missions-batch-1.png)

2. Open the dropdown menu above the table and select the action you want to perform. Then, click Go. 
![Screenshot of Batch Acquisition Missions](images/acquisition-missions-batch-2.png)

A prompt will be shown when trying to delete records. Review and confirm that the data can be deleted before proceeding.
![Screenshot of Batch Acquisition Missions](images/acquisition-missions-batch-3.png)

**Note:** Please be careful when deleting multiple data. 

### Upload Missions

#### Adding a new Upload Mission

1. Login to the admin site with your credentials. You may contact the system administrator if you still don't have an account.

2. Look for the Upload missions module under the MISSIONS_MANAGEMENT subgroup.
![Screenshot of Upload Missions](images/upload-missions.png)

3. You may click the Upload missions button to view and update the current records or the Add button if you want to add a new record.
![Screenshot of Add Acquisition Missions](images/upload-missions-2.png)

4. Add the necessary parameters for the upload mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
![Screenshot of Add Acquisition Missions](images/upload-missions-3.png)

**Note:** Unchecking Generate commands will prevent the command compiler from executing. This means that the error below will not occur and the record will still be successfully saved but without the generated commands.

**Note:** As of April 5, 2020, generating commands for Diwata-1 will result in TLE errors because Diwata-1 has already been decommissioned. Command Compiler tries to fetch the latest TLE values from the server and will return an error if the TLE's epoch date is older than 7 days.

#### Batch operations for Upload Missions

1. Click on the checkbox beside each record to select a record. You may also click the uppermost checkbox to select all the records in the current page.
![Screenshot of Batch Upload Missions](images/upload-missions-batch-1.png)

2. Open the dropdown menu above the table and select the action you want to perform. Then, click Go. 
![Screenshot of Batch Upload Missions](images/upload-missions-batch-2.png)

A prompt will be shown when trying to delete records. Review and confirm that the data can be deleted before proceeding.
![Screenshot of Batch Upload Missions](images/upload-missions-batch-3.png)

**Note:** Please be careful when deleting multiple data. 

### Download Missions

#### Adding a new Download Mission

1. Login to the admin site with your credentials. You may contact the system administrator if you still don't have an account.

2. Look for the Download Missions module under the MISSIONS_MANAGEMENT subgroup.
![Screenshot of Download Missions](images/download-missions.png)

3. You may click the Download Missions button to view and update the current records or the Add button if you want to add a new record.
![Screenshot of Add Download Missions](images/download-missions-2.png)

4. Add the necessary parameters for the mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
![Screenshot of Add Download Missions](images/add-download-mission-1.png)

Some settings are Diwata-2 specific settings so you need to input necessary parameters for the compiler to work properly.

![Screenshot of Add Download Missions](images/add-download-mission-2.png)

Unchecking this box will prevent the command compiler from executing.
![Screenshot of Add Download Missions](images/add-download-mission-3.png)

Add the bands, starting addresses, and number of images to be downloaded, respectively.
![Screenshot of Add Download Missions](images/add-download-mission-4.png)

**Note:** As of April 5, 2020, generating commands for Diwata-1 will result in TLE errors because Diwata-1 has already been decommissioned. Command Compiler tries to fetch the latest TLE values from the server and will return an error if the TLE's epoch date is older than 7 days.

#### Batch operations for Download Missions

1. Click on the checkbox beside each record to select a record. You may also click the uppermost checkbox to select all the records in the current page.
![Screenshot of Batch Download Missions](images/download-missions-batch-1.png)

2. Open the dropdown menu above the table and select the action you want to perform. Then, click Go. 
![Screenshot of Batch Download Missions](images/download-missions-batch-2.png)

A prompt will be shown when trying to delete records. Review and confirm that the data can be deleted before proceeding.
![Screenshot of Batch Download Missions](images/download-missions-batch-3.png)

**Note:** Please be careful when deleting multiple data. 


**Note:** As of April 5, 2020, generating commands for Diwata-1 will result in TLE errors because Diwata-1 has already been decommissioned. Command Compiler tries to fetch the latest TLE values from the server and will return an error if the TLE's epoch date is older than 7 days.

### ACU Log Download Missions

#### Adding a new ACU Log Download Mission

1. Login to the admin site with your credentials. You may contact the system administrator if you still don't have an account.

2. Look for the ACU Log Download Missions module under the MISSIONS_MANAGEMENT subgroup.
![Screenshot of ACU Log Download Missions](images/acu-log-download-missions.png)

3. You may click the ACU Log Download Missions button to view and update the current records or the Add button if you want to add a new record.
![Screenshot of Add ACU Log Download Missions](images/acu-log-download-missions-2.png)

4. Add the necessary parameters for the mission, and then click Save. You may also choose Save and add another if you wish to save another record, or Save and continue editing if you don't want to view and edit the recorded data.
![Screenshot of Add ACU Log Download Missions](images/add-acu-log-download-mission-1.png)

![Screenshot of Add ACU Log Download Missions](images/add-acu-log-download-mission-2.png)

Unchecking this box will prevent the command compiler from executing.
![Screenshot of Add Download Missions](images/add-acu-log-download-mission-3.png)


#### Batch operations for ACU Log Download Missions

1. Click on the checkbox beside each record to select a record. You may also click the uppermost checkbox to select all the records in the current page.
![Screenshot of Batch ACU Log Download Missions](images/acu-log-download-missions-batch-1.png)

2. Open the dropdown menu above the table and select the action you want to perform. Then, click Go. 
![Screenshot of Batch ACU Log Download Missions](images/acu-log-download-missions-batch-2.png)

A prompt will be shown when trying to delete records. Review and confirm that the data can be deleted before proceeding.
![Screenshot of Batch ACU Log Download Missions](images/acu-log-download-missions-batch-3.png)

**Note:** Please be careful when deleting multiple data. 

### Commands and Command Batches

Commands are generated by default whenever missions are created. The list of commands can be found in the Command module of Missions Management module.
![Screenshot of Commands](images/commands.png)

#### Viewing and Editing commands

1. Upon clicking the **Commands** button, you will see all the generated commands from all the different missions (acquisition, download, etc.). Click on the ID to view/edit the content of a command.
![Screenshot of all Commands](images/commands-2.png)

2. Edit the content part of the command and click **Save**.
![Screenshot of all commands](images/commands-3.png)


**Note:** Commands that are created using the Add command module cannot be linked to an existing mission. To be able to link a mission and a command, create a mission and then find the generated command and edit it. Command compiler will execute everytime **any of the three Save** button is pressed. To prevent the command from being replaced, uncheck the **Create commands** button before saving the mission.
![Screenshot of Generated commands from an existing acquisition mission](images/mission-commands.png)


#### Creating command batches
Command batches are a group of commands. It may be a combination of upload, download, acu log download, and acquisition missions. 


1. To create a batch, go to the **command batches** module or click the **Add** button.

![Screenshot of Command Batches 1](images/command-batches-1.png)

If you're already on the list of command batches, you may click the **Add Command batch** on the top right corner of the site.
![Screenshot of Command Batches 2](images/command-batches-2.png)

2. Add the necessary parameters needed.
![Screenshot of Add command batch 1](images/add-command-batch-1.png)

This part will reflect the generated batch after saving the command batch.
![Screenshot of Add command batch 1](images/add-command-batch-2.png)

Supply the ID of the command that will be added to the batch. To select from the list of commands, you may click the search icon beside the text box.

![Screenshot of Add command batch 1](images/add-command-batch-3.png)

 A window with the list of commands will popup. Selecting the ID will supply the text box with the command.

![Screenshot of Add command batch 1](images/add-command-batch-4.png)

Alternatively, you may input the ID of the command in the text box provided.


## Data Management



## Satellite Health Management

### ACU Logs
Attitude Control Unit (ACU) log files contain information recorded onboard the satellite describing the state of its positioning and orientation at a given time. The ground station operators download the log files regularly from the satellites and upload them into a cloud service.

Originally, the contents of the files can be inspected by manually running a depacketer software. This process was tedious and only provided information on-demand.

![Screenshot of Satellite Health Management](images/satellite-health-management-2.png)
<br> Highlighted above are the important components of this module.

1. **Raw Log Files** - raw log files are downloaded from the satellite. An automated depacketer runs at a regular interval to depacket new Raw Log Files. Then the raw files and the depacketed data are uploaded to the operations software
<br>
<br>![Screenshot of List of Raw Log Files](images/raw-log-files.png)
<br> **Raw Log Files Batch Operations**
    * Click on the checkbox beside each record to select a record. You may also click the uppermost checkbox to select all the records in the current page. Open the dropdown menu above the table and select the action you want to perform. Then, click Go. 
<br>![Screenshot of List of Raw Log Files](images/raw-log-files-delete-1.png)
    * A prompt will be shown when trying to delete records. Review and confirm that the data can be deleted before proceeding.
<br>![Screenshot of List of Raw Log Files 2](images/raw-log-files-delete-2.png)

    * **Note:** Always be careful when deleting multiple data. 


2. **CSV Log Files** - the product of depacketing Raw Log Files. These files can be opened in a spreadsheet application or a script for further processing and analysis.
The CSV file can be Group1, Group2, or Group3 logs. Each group contains different columns.
<br>![Screenshot of List of CSV Log Files](images/csv-log-files.png)

* **Note**: Kindly refer to Raw Log Files Batch Operations for the multiple deletion of CSV Log Files.

3. **G1-G3 Logs** - each group contains consolidated data of each row of each group. This allows users to query all data of each group within a specific timeframe instead of downloading individual CSV files.
    * **Group1 Logs**
    <br>![Screenshot of G1 Log](images/g1-log.png)
    * **Group2 Logs**
    <br>![Screenshot of G2 Logs](images/g2-log-1.png)
    ![Screenshot of G2 Logs 2](images/g2-log-2.png)![Screenshot of G2 Logs 3](images/g2-log-3.png)![Screenshot of G2 Logs 4](images/g2-log-4.png)![Screenshot of G2 Logs 5](images/g2-log-5.png)
    * **Group3 Logs**
    <br>![Screenshot of G3 Logs](images/g3-log-1.png)
    ![Screenshot of G3 Logs 2](images/g3-log-2.png)![Screenshot of G3 Logs 3](images/g3-log-3.png)![Screenshot of G3 Logs 4](images/g3-log-4.png)![Screenshot of G3 Logs 5](images/g3-log-5.png)


**Note:** Files generated in this module are uploaded using an automated pipeline. It is not advisable to edit/delete files/data in this module.
