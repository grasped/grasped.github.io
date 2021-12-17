# I. Development

A development environment for the Data Manager API can be setup using Vagrant.

## Dependencies

### 1. Virtualbox

Install the Virtualbox release from https://www.virtualbox.org/wiki/Downloads.

### 2. Vagrant

Download and install Vagrant: https://www.vagrantup.com/downloads.html

### 3. Install vagrant plugins
```
vagrant plugin install vagrant-vbguest
vagrant vbguest

vagrant plugin install vagrant-disksize
```

### 3. (Optional) Vagrant-proxyconf

`vagrant-proxyconf` is a plugin for vagrant for setting up HTTP and HTTPS proxy configuration.

```
vagrant plugin install vagrant-proxyconf
```

## Provision VM

To build a VM for development, run vagrant:

```
cd vagrant
vagrant up
```

Note: If an error of this type occured,
```
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

The error output from the command was:

/sbin/mount.vboxsf: mounting failed with the error: No such device
```

connect to the vagrant machine via ssh and install the dependencies:
```
vagrant ssh

sudo apt-get update && \
sudo apt-get install -y linux-image-amd64 linux-headers-amd64 && \
sudo /sbin/rcvboxadd quicksetup all && \
logout
```

Reload virtual machine:
```
vagrant reload
```

## Running within Vagrant

### 1. Activate the local environment

```
source venvs/operations-software/bin/activate
```

### 2. Run tests
```
python manage.py test
```

or run a specific test...
```
python manage.py test apps.data_management.tests.test_captures.CaptureTests
python manage.py test apps.missions_management.tests.test_missions.TestMissionsAPI
```

### 3. Run server
```
python manage.py runserver 0.0.0.0:8000
```

## Running within Vagrant behind Nginx
To address connection issues due to CSRF, django can be run behind Nginx. To do this, run:
```
./uwsgi-vagrant.sh
```

The app will then be available through http://localhost:8001.


# II. Deployment

The Data Manager API is deployed using Ansible. Configurations for deployment is located under `/vagrant/inventory`.

## SSHConfig

```
Host phl-microsat-bastion
    User benjiao
    Hostname 202.92.131.229
    Port 2231
    IdentityFile ~/.ssh/phl-microsat

Host pmsat-dev
    User benjiao
    HostName 10.37.166.48
    IdentityFile ~/.ssh/phl-microsat
    ProxyCommand ssh -xaqW%h:22 phl-microsat-bastion

Host dm-prod
    User benjiao
    Hostname 192.168.27.41
    ProxyCommand ssh -xaqW%h:22 phl-microsat-bastion
```

## Install Python on Deployment Machine

Python is required by Ansible.

```
sudo apt install python-minimal
```

## Create a User

Before running any setup command, we must first create a user for our application.

```
cd vagrant
ansible-playbook playbooks/user.yml -i inventories/<host>
```

NOTE: Copy the key returned by the task. `"ssh-rsa ...`". Add this as a deploy key on Gitlab repos for:

- Data Manager API (https://gitlab.com/phl-microsat-dpad/operations-software/settings/repository)
- Image Uploader 2 (https://gitlab.com/phl-microsat-dpad/image-uploader2/settings/repository)
- Image Depacketer (https://gitlab.com/phl-microsat-dpad/image-depacketer/settings/repository)


This will enable our ansible script to pull the source from our private repository.

## Run Provisioning Scripts

```
cd vagrant
ansible-playbook playbooks/provision.yml -i inventories/<host>
```

# III. API

## Missions Management

### 1. Acquisition Missions

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/missions<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get mission by name | `?name=sample` |
| created_time | Display created missions in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| capture_time | Display captured missions in date range | `?from:capture_time=2019-12-01&to:capture_time=2019-12-25` |
| satellite | Filter missions by satellite | `?satellite=Diwata-1` |
| status | Filter missions by status | `?status=Pending` |
| pointing_mode | Filter missions by pointing mode | `?pointing_mode=Target Pointing` |
| capture_groups | Filter missions by capture_groups | `?capture_groups=D2_SMI` |
| captures | Filter missions by captures it contains | `?captures=D2_SMI` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&status=Pending&pointing_mode=Target Pointing` |
| search | Display data with related keyword | `search=keyword` |

### 2. Download Missions

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/download_missions<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get download mission by name | `?name=sample` |
| created_time | Display created download missions in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| capture_time | Display captured download missions in date range | `?from:capture_time=2019-12-01&to:capture_time=2019-12-25` |
| aos_time | Display range of download missions by aos time | `?from:aos_time=2019-12-01&to:aos_time=2019-12-25` |
| los_time | Display range of download missions by los time | `?from:los_time=2019-12-01&to:los_time=2019-12-25` |
| eclipse_time | Display range of download missions by eclipse time | `?from:eclipse_time=2019-12-01&to:eclipse_time=2019-12-25` |
| start_of_download_time | Display range of download missions by start of download time | `?from:start_of_download_time=2019-12-01&to:start_of_download_time=2019-12-25` |
| aru_on_time | Display range of download missions by aru on time | `?from:aru_on_time=2019-12-01&to:aru_on_time=2019-12-25` |
| aru_off_time | Display range of download missions by aru off time | `?from:aru_off_time=2019-12-01&to:aru_off_time=2019-12-25` |
| satellite | Filter download missions by satellite | `?satellite=Diwata-1` |
| status | Filter download missions by status | `?status=Pending` |
| receiving_station | Filter download missions by receiving station | `?receiving_station=ASTI` |
| day_download | Filter download missions by day download | `?day_download=True` |
| flash_memory | Filter download missions by flash memory | `?flash_memory=True` |
| aru_on | Filter download missions by aru on | `?aru_on=True` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&status=Pending&receiving_station=ASTI` |
| search | Display data with related keyword | `search=keyword` |

### 3. Upload Missions

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/upload_missions<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get upload mission by name | `?name=sample` |
| created_time | Display created upload missions in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| execution_time | Display range of download missions by execution time | `?from:execution_time=2019-12-01&to:execution_time=2019-12-25` |
| satellite | Filter upload missions by satellite | `?satellite=Diwata-1` |
| status | Filter upload missions by status | `?status=Pending` |
| receiving_station | Filter upload missions by receiving station | `?receiving_station=ASTI` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&status=Pending&receiving_station=ASTI` |
| search | Display data with related keyword | `search=keyword` |

### 4. ACU Log Download Missions

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/acu_log_download_missions<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get ACU Log Download mission by name | `?name=sample` |
| created_time | Display created missions in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| execution_time | Display captured missions in date range | `?from:execution_time=2019-12-01&to:execution_time=2019-12-25` |
| satellite | Filter download missions by satellite | `?satellite=Diwata-1` |
| status | Filter download missions by status | `?status=Pending` |
| receiving_station | Filter download missions by receiving station | `?receiving_station=ASTI` |
| acu_log_segment | Filter by acu log segment | `?acu_log_segment=SEG-A` |
| number_of_sets | Filter by number of sets | `?number_of_sets=192` |
| record_interval_seconds | Filter by record of interval seconds | `?record_interval_seconds=5` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&status=Pending&receiving_station=ASTI` |
| search | Display data with related keyword | `search=keyword` |

### 5. Targets

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/targets<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get Targets by name | `?name=sample` |
| created_time | Display targets in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| classification | Display targets by classification | `?classification=5` |
| required_bands | Display targets by contained required band | `?required_bands=450` |
| purpose | Filter targets by purpose | `?purpose=Agriculture` |
| search | Display data with related keyword | `search=keyword` |

### 6. SHU Address Tracker

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/shu_tracker<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| satellite | Get SHU Addresses by satellite | `?satellite=Diwata-1` |
| payload | Get SHU Addresses by payload | `?payload=HPT` |
| band | Get SHU Addresses by band | `?band=HPT-G` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&payload=SMI` |

### 7. Command Batches

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/command_batches<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get Targets by name | `?name=sample` |
| created_time | Display command batches in date range depending on created_time | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| upload_time | Display command batches in date range depending on upload_time | `?from:upload_time=2019-12-01&to:upload_time=2019-12-25` |
| status | Filter command batches batches by status | `?status=Pending` |
| satellite | Filter command batches by satellite | `?satellite=Diwata-1` |
| commands_list | Find command_batches by section_number contained | `commands_list=D1-2` |
| remarks | Search command batches that contain remarks | `remarks=sample` |
| description | Search command batches that contain description | `description=sample` |
| search | Display data with related keyword | `search=keyword` |

### 8. ACU Log Segments

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/acu_log_segments<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get ACU Log Segment by id | `?id=1` |
| name | Get ACU Log Segment by name | `?name=SEG-A` |
| max_number_of_sets | Get ACU Log Segment by max number of sets | `?max_number_of_sets=192` |
| starting_address | Get ACU Log Segment by starting address | `?starting_address=66C000` |

### 9. Mission Types

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/mission_types<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get Mission Type by id | `?id=1` |
| name | Get Mission Type by name | `?name=Routine ops` |

### 10. Command Types

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/command_types<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get Command Type by id | `?id=1` |
| name | Get Command Type by name | `?name=Acquisition` |
| full_name | Get Command Type by full_name (satellite - name) | `?full_name=Diwata-1 - Acquisition` |
| satellite | Get Command Type by satellite | `?satellite=Diwata-2` |

### 11. Stats

#### a. Mission Status Stats

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/mission_status_stats<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| satellite | Mission status stats for the specified satellite. Default is "ALL" which returns the total stats for all satellites | `?satellite=Diwata-1` |
| created_time | Display missions status stats in date range depending on created_time | `?from:created_time=2019-01-01&to:created_time=2020-01-09` |

#### b. Upload Failure Stats

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/upload_failure_stats<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| satellite | Upload failure stats for the specified satellite. Default is "ALL" which returns the total stats for all satellites | `?satellite=Diwata-1` |
| upload_time | Display upload failure stats in date range depending on upload_time | `?from:upload_time=2019-01-01&to:upload_time=2020-01-09` |

#### c. Mission Type Stats

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/mission_type_stats<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| satellite | Mission type stats for the specified satellite. Default is "ALL" which returns the total stats for all satellites | `?satellite=Diwata-1` |
| created_time | Display mission type stats in date range depending on created_time | `?from:created_time=2019-01-01&to:created_time=2020-01-09` |


### 11. Commands

`https://api.ops.phl-microsat.upd.edu.ph/missions_management/commands<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| created_time | Display commands in date range depending on created_time | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| execution_time | Display commands in date range depending on execution_time | `?from:execution_time=2019-12-01&to:execution_time=2019-12-25` |
| command_type | Filter commands by command_type | `?command_type=Acquisition` |
| command_batch | Filter commands by command_batch | `?command_batch=1` |
| section_number | Find commands by section_number | `section_number=D1-2` |

| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?command_type=Acquisition&command_batch=Taal` |
| search | Display data with related keyword | `search=keyword` |

Example POST/PATCH body:
```
{
    'content': "TEST CONTENT",
    'section_number': '111',
    'execution_time': "2020-02-25T03:00:00Z",
    'command_type': 2,
    'command_batch': 1
}
```


## Data Management Module
### 1. Captures

**GET**
`https://api.ops.phl-microsat.upd.edu.ph/data_management/captures<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| capture_id | Filter captures by capture id | `?capture_id=D1_SMI_2019-12-11T063459166_N730` |
| gain | Filter captures by gain (range) | `?from:gain=100&to:gain=150` |
| pdo |  Filter captures by PDO (range) | `?from:pdo=0&from:pdo=5` |
| exposure_time | Filter captures by exposure time (range) | `?from:exposure_time=500&from:exposure_time=1000` |
| packet_loss | Filter captures by packet loss | `?to:packet_loss=0` |
| wavelength |  Filter captures by wavelength or list of wavelength | `?wavelength=720,680` |
| band |  Filter captures by band or list of bands | `?band=SMI-N,SMI-V` |
| satellite | Filter captures by satellite or list of satellites | `?satellite=Diwata-1,Diwata-2` |
| processing_level | Filter captures by processing level or list of processing level | `?processing_level=L1C` |
| product:l1a | Include/exclude captures with L1A products | `?product:l1a=true` |
| product:l1b | Include/exclude captures with L1B products | `?product:l1b=false` |
| product:l1c | Include/exclude captures with L1C products | `?product:l1c=true` |
| image_tags | Include/exclude captures based on image tag/s | `image_tags=Urban,Coastal` |

**PATCH**
https://api.ops.phl-microsat.upd.edu.ph/data_management/captures/D1_SMI_2019-12-11T063459166_N730/

Sample Body: 
```
{
    "satellite": "Diwata-2",
    "processing_level": "L1C"
}
```

### 2. Missions Coverage Map

`https://api.ops.phl-microsat.upd.edu.ph/data_management/missions_coverage<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get mission by name | `?name=sample` |
| created_time | Display created captures map in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| capture_time | Display captures map in date range | `?from:capture_time=2019-12-01&to:capture_time=2019-12-25` |
| payload | Filter capture map by payload/s | `?payload=SMI,ERC` |
| satellite | Filter capture map by satellite | `?satellite=Diwata-1` |
| receiving_station | Filter capture map by receiving_station | `?receiving_station=ASTI` |
| processing_level | Filter capture map by processing_level/s | `?processing_level=Raw,L1A` |
| image_quality | Filter capture map by image_quality | `?image_quality=excellent` |
| is_published | Filter capture map by is_published | `?is_published=true` |
| image_tags | Filter capture map by tags | `?image_tags=Taal,Mayon` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-1&payload=SMI,ERC` |


### 3. Image Tags

`https://api.ops.phl-microsat.upd.edu.ph/data_management/image_tags<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get image tags by name | `?name=Urban` |
| description | Get image tags by description | `?description=Urban` |
| date_added | Display image tags in date range | `?from:date_added=2019-12-01&to:date_added=2019-12-25` |
| added_by | Get image tags by added_by | `?added_by=sample` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=Urban&description=Urban` |
| search | Display data with related keyword | `search=keyword` |


### 4. Capture Image Tags
**GET** 
```
https://api.ops.phl-microsat.upd.edu.ph/data_management/captures/<capture_id>/image_tags/
```
**POST** 
```
https://api.ops.phl-microsat.upd.edu.ph/data_management/captures/<capture_id>/image_tags/

{"image_tags": ["Urban"]}
```
**DELETE** 
```
https://api.ops.phl-microsat.upd.edu.ph/data_management/captures/<capture_id>/image_tags/

{"image_tags": ["Urban"]}
```


### 5. GCP Files / GCP Json

`https://api.ops.phl-microsat.upd.edu.ph/data_management/gcp_files<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get GCP files by name | `?name=D2_ERC_2019-08-18T050856064_-gcps.points` |
| uploaded_by | Get GCP files based on uploader | `?uploaded_by=admin` |
| upload_time | Display GCP files in date range | `?from:upload_time=2019-12-01&to:upload_time=2019-12-25` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=Urban&description=Urban` |


**Search GCP File via md5sum** 
```
`https://api.ops.phl-microsat.upd.edu.ph/data_management/gcp_files/md5sum/<md5sum>`
```
**Get Capture GCPs in CSV format** 
```
`https://api.ops.phl-microsat.upd.edu.ph/data_management/captures/<capture_id>/gcps.points`
```
**POST**

   **1) GCP File** - `https://api.ops.phl-microsat.upd.edu.ph/data_management/gcp_files/`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | Name of the object to be saved. Default is the filename. | No |
| gcp_file | The file to be uploaded | Yes |
| capture_id | The capture_id for the associated gcp_file. If nothing is given, it will try to extract capture_id from the filename. | No |


   **2) GCP Json** - `https://api.ops.phl-microsat.upd.edu.ph/data_management/gcp_files/`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | Name of the object to be saved. Default is `<capture_id>-gcps.points`. | No |
| json_string | The data of the gcp points in json string format | Yes |
| capture_id | The capture_id for the associated gcp points. Required when uploading json string. | Yes |


### 6. Telemetry Files

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/telemetry_files<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| ordering | Set ordering of received viewset. Default is 'id' | `?ordering=date_created` |
| name | Get telemetry files matching the name |  `?name=F20190711131248.bin`, `?name=F2019` |
| telemetry_type | Get telemetry files matching the telemetry type |  `?telemetry_type=XTLM` |
| receiving_station | Get telemetry files matching the receiving station |  `?receiving_station=ASTI` |
| satellite | Get telemetry files matching the satellite |  `?satellite=Diwata-2` |
| uploaded_by | Get telemetry files matching the name of the uploader | `?uploaded_by=admin`, `?uploaded_by=adm` |
| upload_time | Get telemetry files given date range of upload time. | `?from:upload_time=2019-12-01&to:upload_time=2019-12-25` |
| created_time | Get telemetry files given date range of created time. | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=F2019&satellite=Diwata-2` |

GET by id, md5sum:
```
`https://api.ops.phl-microsat.upd.edu.ph/data_management/telemetry_files/<id>`
`https://api.ops.phl-microsat.upd.edu.ph/data_management/telemetry_files/md5sum/<md5sum>`
```
Sample returned data:
```
{
    "id": 14,
    "telemetry_type": "XTLM",
    "receiving_station": "ASTI",
    "satellite": "Diwata-1",
    "link": "http://localhost:8000/data_management/telemetry_files/14",
    "uploaded_by": "admin",
    "name": "F20190711131248.bin",
    "file": "http://localhost:8000/media/telemetry_files/F20190711131248.bin",
    "date_uploaded": "2020-09-11T15:39:06.545321Z",
    "date_created": "2020-02-27T21:11:50Z",
    "md5sum": "d675554f6038d3b20385c8741bffb26a"
}
```

**POST**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/telemetry_files`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | Telemetry file name | No. Will get default file name. |
| telemetry_type | Telemetry type. (XTLM, QLTM) |  Yes |
| satellite | What satellite? (Diwata-1, Diwata-2, Maya-1) | Yes |
| receiving_Station | What GRS was used? (ASTI, TU, HAKODATE, KIRUNA) | Yes |
| file | The telemetry file. Usually the .bin file | Yes |
| date_created | Created date of the TLM file | Yes |
| md5sum | The md5sum of the telemetry file. | Yes |

**PATCH**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/telemetry_files/<id>`

Sample Patch Body: 
```
{
    "name": "F20190711131248.bin",
    "receiving_station": "KIRUNA"
}
```


### 7. Raw Images

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/raw_images<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| ordering | Set ordering of received viewset. Default is 'id' | `?ordering=date_created` |
| name | Get raw images matching the name |  `?name=ERC_CAP200825_061646_284740_DEC200828_011732.rg3`, `?name=CAP200825` |
| from_telemetry_file | Get raw images from a telemetry file |  `?from_telemetry_file=F20190711131248.bin` |
| satellite | Get raw images matching the satellite |  `?satellite=Diwata-2` |
| uploaded_by | Get raw images matching the name of the uploader | `?uploaded_by=admin`, `?uploaded_by=adm` |
| upload_time | Get raw images given date range of upload time. | `?from:upload_time=2019-12-01&to:upload_time=2019-12-25` |
| date_extracted | Get raw images given date range of extraction time from the TLM file. | `?from:date_extracted=2019-12-01&to:date_extracted=2019-12-25` |
| depacketer_version | Get raw images with the depacketer version | `?depacketer_version=1.3.4` 
| packet_loss_min | Get raw images with minimum packet loss | `?packet_loss_min=5.0`  |
| packet_loss_max | Get raw images with maximum packet loss | `?packet_loss_max=25.0` | 
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=ERC_CAP200825_061646_284740_DEC200828_011732.rg3&satellite=Diwata-2` |

GET by id, md5sum:
```
`https://api.ops.phl-microsat.upd.edu.ph/data_management/raw_images/<id>`
`https://api.ops.phl-microsat.upd.edu.ph/data_management/raw_images/md5sum/<md5sum>`
```
Sample returned data:
```
{
    "id": 99,
    "name": "ERC_CAP200825_061646_284740_DEC200828_011732.rg3",
    "from_telemetry_file": {
        "id": 15,
        "telemetry_type": "XTLM",
        "receiving_station": "ASTI",
        "satellite": "Diwata-2",
        "link": "http://localhost:8000/data_management/telemetry_files/15",
        "uploaded_by": "admin",
        "name": "F20200828011732.bin",
        "file": "http://localhost:8000/media/telemetry_files/F20200828011732_pz2OfR8.bin",
        "date_uploaded": "2020-09-11T15:40:29.665975Z",
        "date_created": "2020-08-28T01:47:32Z",
        "md5sum": "681710445cda11d1ab46dba1e46c6f8d"
    },
    "satellite": {
        "id": 2,
        "name": "Diwata-2",
        "short_name": "D2",
        "created_time": "2017-01-09T04:54:51.175000Z",
        "color": "",
        "catalog_no": "43678",
        "avatar_initials": "D2",
        "status": 1
    },
    "link": "http://localhost:8000/data_management/raw_images/99",
    "uploaded_by": "test@phl-microsat.xyz",
    "file": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200828_011732_rvd6ZgP.rg3",
    "thumbnail": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200828_011732_1XeMEVH.png",
    "packet_loss": 0.0,
    "date_uploaded": "2020-09-11T15:40:59.839378Z",
    "date_extracted": "2020-09-11T23:40:54Z",
    "depacketer_version": "1.3.4",
    "md5sum": "8772786b002c16f55a468cfc01c75403"
}
```

**POST**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/raw_images`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | Raw Image file name | No. Will get default file name. |
| from_telemetry_file | Raw image parent telemetry file |  No |
| satellite | Diwata-1, Diwata-2, Maya-1 | Yes |
| file | Raw image. Usually the .rg3 file | Yes |
| thumbnail | Thumbnail file. Usually a .png file | Yes |
| date_extracted | Depacketing time of the TLM file | Yes |
| packet_loss | Packet loss of the raw image | No |
| depacketer_version | Depacketer version used for the image | No |
| md5sum | The md5sum of the raw image. | No. Computed automatically. |

**PATCH**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/raw_images/<id>`

Sample Patch Body: 
```
{
    "packet_loss": 0.345,
    "depacketer_version": "1.3.4"
}
```


### 8. Merged Raw Images

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/merged_raw_images<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| ordering | Set ordering of received viewset. Default is 'id' | `?ordering=date_created` |
| name | Get merged raw images matching the name |  `?name=ERC_CAP200825_061646_284740.bin`, `?name=284740.bin` |
| satellite | Get merged raw images matching the satellite |  `?satellite=Diwata-2` |
| uploaded_by | Get merged raw images matching the name of the uploader | `?uploaded_by=admin`, `?uploaded_by=adm` |
| date_created | Get merged raw images given date range of created date |  `?from:date_created=2019-12-01&to:date_created=2019-12-25` |
| date_uploaded | Get merged raw images given date range of uploaded date |  `?from:date_uploaded=2019-12-01&to:date_uploaded=2019-12-25` |
| date_depacketed | Get merged raw images given date range of depacketed date |  `?from:date_depacketed=2019-12-01&to:date_depacketed=2019-12-25` |
| capture_time | Get merged raw images given date range of capture time | `?from:capture_time=2019-12-01&to:capture_time=2019-12-25` |
| image_depacketer_version | Get merged raw images matching the image depacketer version | `?image_depacketer_version=1.3.4` |
| shu_address | Get merged raw images matching the hex shu address | `?shu_address=033` |
| capture_id | Get merged raw images matching the capture id | `?capture_id=D2_ERC_2020-08-25T061646028_` |
| raw_images | Get merged raw images containing one or more raw image(s) | `?raw_images=ERC_CAP200825_061646_284740_DEC200828_011732.rg3,ERC_CAP200825_061646_284740_DEC200829_010945.rg3` |
| depacketed | Get merged raw images that are depacketed/not depacketed | `?depacketed=true`, `?depacketed=false` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=ERC_CAP200825_061646_284740.bin&satellite=Diwata-2` |

GET by id, md5sum:
```
`https://api.ops.phl-microsat.upd.edu.ph/data_management/merged_raw_images/<id>`
`https://api.ops.phl-microsat.upd.edu.ph/data_management/merged_raw/md5sum/<md5sum>`
```
Sample returned data:
```
{
    "id": 105,
    "raw_images_details": [
        {
            "id": 99,
            "name": "ERC_CAP200825_061646_284740_DEC200828_011732.rg3",
            "from_telemetry_file": {
                "id": 15,
                "telemetry_type": "XTLM",
                "receiving_station": "ASTI",
                "satellite": "Diwata-2",
                "link": "http://localhost:8000/data_management/telemetry_files/15",
                "uploaded_by": "admin",
                "name": "F20200828011732.bin",
                "file": "http://localhost:8000/media/telemetry_files/F20200828011732_pz2OfR8.bin",
                "date_uploaded": "2020-09-11T15:40:29.665975Z",
                "date_created": "2020-08-28T01:47:32Z",
                "md5sum": "681710445cda11d1ab46dba1e46c6f8d"
            },
            "satellite": {
                "id": 2,
                "name": "Diwata-2",
                "short_name": "D2",
                "created_time": "2017-01-09T04:54:51.175000Z",
                "color": "",
                "catalog_no": "43678",
                "avatar_initials": "D2",
                "status": 1
            },
            "link": "http://localhost:8000/data_management/raw_images/99",
            "uploaded_by": "test@phl-microsat.xyz",
            "file": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200828_011732_rvd6ZgP.rg3",
            "thumbnail": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200828_011732_1XeMEVH.png",
            "packet_loss": 0.0,
            "date_uploaded": "2020-09-11T15:40:59.839378Z",
            "date_extracted": "2020-09-11T23:40:54Z",
            "depacketer_version": "1.3.4",
            "md5sum": "8772786b002c16f55a468cfc01c75403"
        },
        {
            "id": 100,
            "name": "ERC_CAP200825_061646_284740_DEC200829_010945.rg3",
            "from_telemetry_file": {
                "id": 15,
                "telemetry_type": "XTLM",
                "receiving_station": "ASTI",
                "satellite": "Diwata-2",
                "link": "http://localhost:8000/data_management/telemetry_files/15",
                "uploaded_by": "admin",
                "name": "F20200828011732.bin",
                "file": "http://localhost:8000/media/telemetry_files/F20200828011732_pz2OfR8.bin",
                "date_uploaded": "2020-09-11T15:40:29.665975Z",
                "date_created": "2020-08-28T01:47:32Z",
                "md5sum": "681710445cda11d1ab46dba1e46c6f8d"
            },
            "satellite": {
                "id": 2,
                "name": "Diwata-2",
                "short_name": "D2",
                "created_time": "2017-01-09T04:54:51.175000Z",
                "color": "",
                "catalog_no": "43678",
                "avatar_initials": "D2",
                "status": 1
            },
            "link": "http://localhost:8000/data_management/raw_images/100",
            "uploaded_by": "admin",
            "file": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200829_010945.rg3",
            "thumbnail": "http://localhost:8000/media/raw_images/ERC_CAP200825_061646_284740_DEC200829_010945.png",
            "packet_loss": 0.0,
            "date_uploaded": "2020-09-14T06:25:15.517844Z",
            "date_extracted": "2020-09-14T06:25:14Z",
            "depacketer_version": "",
            "md5sum": "6882629a1579861e89b3cb32db62d746"
        }
    ],
    "link": "http://localhost:8000/data_management/merged_raw_images/105",
    "uploaded_by": "admin",
    "satellite": {
        "id": 2,
        "name": "Diwata-2",
        "short_name": "D2",
        "created_time": "2017-01-09T04:54:51.175000Z",
        "color": "",
        "catalog_no": "43678",
        "avatar_initials": "D2",
        "status": 1
    },
    "name": "ERC_CAP200825_061646_284740.bin",
    "date_created": "2020-09-11T15:41:12.239393Z",
    "merged_rg3_image": "http://localhost:8000/media/merged_files/ERC_CAP200825_061646_284740_rHXpDhC.bin",
    "md5sum_rg3_image": "8772786b002c16f55a468cfc01c75403",
    "date_uploaded": "2020-09-11T15:41:12.238903Z",
    "file_tif": "http://localhost:8000/media/merged_files/D2_ERC_2020-08-25T061646028_000.000_ErMDLrz.tif",
    "file_full_tif": "http://localhost:8000/media/merged_files/D2_ERC_2020-08-25T061646028_000.000_full_ery2hKb.tif",
    "thumbnail": "http://localhost:8000/media/merged_files/D2_ERC_2020-08-25T061646028_000.000-thumb_8wxefhO.jpg",
    "meta_json": "http://localhost:8000/media/merged_files/D2_ERC_2020-08-25T061646028_000.000_TOJIHxn.json",
    "meta_csv": "http://localhost:8000/media/merged_files/D2_ERC_2020-08-25T061646028_000.000_LahSeq8.csv",
    "date_depacketed": "2020-09-11T15:41:19.595800Z",
    "image_depacketer_version": "1.3.4",
    "shu_address": "033",
    "capture_id": "D2_ERC_2020-08-25T061646028_",
    "capture_time": "2020-08-25T06:16:46.028047Z",
    "shutter_mode": 0.0,
    "dark_count": 1028.57032203907,
    "temperature_smi": 15.255404725993,
    "temperature_hpt": 14.2498743086979,
    "temperature_mfc": 12.2388134741077,
    "temperature_wfc": 12.2388134741077,
    "temperature_erc": 14.2498743086979,
    "temperature_lctf_vis": -17.0,
    "temperature_lctf_nir": -17.0,
    "temperature_shu1": 26.3162393162394,
    "temperature_shu2": 23.299648064354,
    "current_hpt_vis": 0.0,
    "current_hpt_nir": 0.0,
    "current_smi_vis_nir": 0.0,
    "current_lctf_3_3v": 0.0,
    "current_lctf_15v": 0.00359776938298255,
    "current_lctf_neg_15v": 0.0,
    "current_3_3v": 0.0727499480357514,
    "current_digital_5v": 0.0431890818001209,
    "current_analog_5v": 0.00474536377958734,
    "voltage_3_3v": 3.23529411764706,
    "voltage_5v": 5.05840634125991,
    "voltage_15v": 14.1960784313725,
    "voltage_neg_15v": -13.8039215686275,
    "voltage_1_8v": 1.84313725490196,
    "raw_images": [
        99,
        100
    ]
}
```

**POST**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/merged_raw_images`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | Merged raw images file name | Yes |
| satellite | Diwata-1, Diwata-2, Maya-1 | Yes |
| merged_rg3_image | Merged raw image. Usually the .bin file | Yes |
| raw_images_list | A .txt file containing the raw images combined to form the merged raw image | Yes |
| md5sum_rg3_image | The md5sum of the merged raw image. | No. Computed automatically. |
| file_tif | The .tif output file of the image depacketer | No |
| file_full_tif | The -full.tif output file of the image depacketer | No |
| thumbnail | The .jpg output file of the image depacketer | No
| meta_json | The .json output file of the image depacketer | No
| meta_csv | The .csv file of the image depacketer | No 
| date_depacketed | Depacketing time of the merged raw image | No |
| image_depacketer_version | The version of the image depacketer used if the image is depacketed. | No |

**PATCH**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/merged_raw_images/<id>`

Sample Patch Body: 
```
{
    "file_tif": open('file.tif', 'rb'),
    "file_full_tif": open('file-full.tif', 'rb'),
    "thumbnail": open('thumbnail.jpg', 'rb'),
    "meta_json": open('meta.json', 'r'),
    "meta_csv": open('meta.csv', 'r'),
}
```

### 9. Capture Groups
**GET**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_groups<parameter/s>
https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_groups/<capture_group_id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| capture_group_id | Capture Group ID | `/D2_SMI_2018-12-04T045918_2018-12-04T235959` |
| description | Sort by description | `?description=in Palawan` |
| payload | Sort by payload | `?payload=SMI` |
| created_time | Get capture group products given date range of upload time. | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| added_by | Sort by uploader | `?added_by=admin` |
| mission | Sort by capture groups the group is included | `?mission=In China` |
| capture_group_products | Sort capture groups by products it contains | `?capture_group_products=D2_HPT` |
| captures | Sort capture groups by captures it contains | `?captures=D2_HPT` |

Sample returned body: 
```
{
    "id": 7,
    "mission": "Major Fleet, China",
    "thumbnail": "http://localhost:8000/media/capture_group_products_uploaded_files/main_menu_parts.png",
    "capture_group_id": "D2_HPT_2019-06-04T050736022",
    "description": "Went to China.",
    "created_time": "2021-03-18T15:46:27Z",
    "payload": "HPT",
    "added_by": "admin",
    "captures": [
        {
            "capture_id": "D2_HPT_2019-06-04T050736022_N",
            "link": "http://localhost:8000/data_management/captures/D2_HPT_2019-06-04T050736022_N"
        },
        {
            "capture_id": "D2_HPT_2019-06-04T050736022_B",
            "link": "http://localhost:8000/data_management/captures/D2_HPT_2019-06-04T050736022_B"
        },
        {
            "capture_id": "D2_HPT_2019-06-04T050736022_G",
            "link": "http://localhost:8000/data_management/captures/D2_HPT_2019-06-04T050736022_G"
        },
        {
            "capture_id": "D2_HPT_2019-06-04T050736022_R",
            "link": "http://localhost:8000/data_management/captures/D2_HPT_2019-06-04T050736022_R"
        }
    ],
    "capture_group_products": [
        {
            "capture_group_product_id": "TEST",
            "product_type": "Stacked",
            "link": "http://localhost:8000/data_management/capture_group_products/TEST"
        },
        {
            "capture_group_product_id": "TEST_2",
            "product_type": "Stacked",
            "link": "http://localhost:8000/data_management/capture_group_products/TEST_2"
        },
        {
            "capture_group_product_id": "D2_HPT_2019-06-04T050736022",
            "product_type": "Stacked",
            "link": "http://localhost:8000/data_management/capture_group_products/D2_HPT_2019-06-04T050736022"
        }
    ]
}
```
**POST**
`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_groups/`
| Parameter | Description | Required |
| ----- | ----- | ----- |
| capture_group_id | ID | Yes |
| description | Description of the group | No |
| payload | Payload type of the capture | Yes |
| mission | The mission the group is included | Yes |

Sample POST body: 
```
{
    'capture_group_id': 'D2_SMI_2018-12-04T045918_2018-12-04T235959_TEST',
    'description': 'This is just a test capture group.',
    'payload': 'SMI',
    'mission': 'Major Fleet, China'
}
```

**PATCH**
`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_groups/<capture_group_id>`

Same as POST except capture_group_id


### 10. Capture Group Products
**GET**

`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_group_products<parameter/s>
https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_group_products/<capture_group_product_id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| capture_group_product_id | Capture Group Product ID | `/D2_SMI_2018-12-04T045918_2018-12-04T235959` |
| capture_group_product_id | Sort by capture group product ID | `?capture_group_product_id=D2_SMI` |
| uploaded_by | Sort by uploader | `?uploaded_by=admin` |
| description | Sort by description | `?description=in Palawan` |
| upload_time | Get capture group products given date range of upload time. | `?from:upload_time=2019-12-01&to:upload_time=2019-12-25` |
| capture_group | Sort by capture group | `?capture_group=D2_SMI_2018-12-04T045918_2018-12-04T235959` |
| product_type | Sort by uploader | `?product_type=Mosaic` |
| image_tags | Include/exclude captures based on image tag/s | `image_tags=Urban,Coastal` |

Sample returned body: 
```
{
    "id": 12,
    "uploaded_by": "admin",
    "product_type": "Stacked",
    "capture_group": "D2_HPT_2019-06-04T050736022",
    "file": null,
    "thumbnail": "http://localhost:8000/media/capture_group_products_uploaded_files/done_450.PNG",
    "image_tags": ["Urban", "Vegetation"],
    "capture_group_product_id": "D2_HPT_2019-06-04T050736022",
    "md5sum": "",
    "description": "Captured in Palawan",
    "footprint": null,
    "upload_time": "2021-03-18T16:01:15.532901Z"
}
```

**POST**
`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_group_products/`
| Parameter | Description | Required |
| ----- | ----- | ----- |
| capture_group_product_id | ID | Yes |
| description | Description of the capture group product | No |
| file | The capture group product image in .TIF | Yes |
| thumbnail | The capture group product thumbnail in .PNG | Yes |
| product_type | The type of product | Yes |
| capture_group | The capture group it belongs to | Yes |
| image_tags | Tags of the product | No |

Sample post body: 
```
{
    'capture_group_product_id': 'D2_SMI_2018-12-04T045918_2018-12-04T235959_TEST',
    'description': 'This is just a test capture group.',
    'file': open(.../D2_SMI_2018-12-04T045918_2018-12-04T235959.tif','rb'),
    'thumbnail': open(.../D2_SMI_2018-12-04T045918_2018-12-04T235959_thumb.png','rb'),
    'product_type': 'Mosaic',
    'capture_group': 'D2_SMI_2018-12-04T045918_2018-12-04T235959',
    'image_tags': ["Urban", "Vegetation"]
}
```

**PATCH**
`https://api.ops.phl-microsat.upd.edu.ph/data_management/capture_group_products/<capture_group_product_id>`

Same to POST except capture_group_product_id


## Satellite Health Management
### 1. Raw Log File Types

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_file_types<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_file_types/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get raw log file types matching the name |  `?name=RG3` |
| id | Get raw log file types with id | `?id=1` |
| description | Get raw log file types matching the description |  `?description=rg3` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=RG3&description=.rg3` |


### 2. Csv File Log Types

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_file_types<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_file_types/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get csv log file types matching the name |  `?name=G1` |
| id | Get csv log file types with id | `?id=1` |
| description | Get csv log file types matching the description |  `?description=Group 1` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=G1&description=Group 1` |


### 3. Raw Log Files

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_files<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_files/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get raw log files with id | `?id=171` |
| name | Get raw log files matching the name |  `?name=ACU__DEC200702_015135_RAW_659x494.rg3` |
| satellite | Get raw log files matching the satellite |  `?satellite=Diwata-2` |
| receiving_station | Get raw log files matching the receiving station | `?receiving_station=ASTI` | 
| raw_log_type | Get raw log files matching the raw log file type | `?raw_log_type=RG3` | 
| date_created | Get raw log files given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| date_uploaded | Get raw log files given date range of uploaded date | `?from:date_uploaded=2020-01-01&to:date_uploaded=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=ACU__DEC200702_015135_RAW_659x494.rg3&raw_log_type=RG3` |

GET by md5sum

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_files/md5sum/<md5sum>`

**POST**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/raw_log_files/`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | File name for the raw log file |  No. Will get default file name |
| file | The .rg3 or .qltlm file | Yes |
| raw_log_type | They type of file (RG3/QLTLM) | No. Will get file extension |
| satellite | Diwata-1, Diwata-2, Maya-1 | Yes |
| receiving_station | ASTI, HAKODATE, KIRUNA, TU | Yes |
| date_created | UTC time when the raw log file was created | Yes |
| uploaded_by | The user who uploaed the file | No |
| md5sum | The md5sum of the raw log file | No. Automatically computed |


### 4. CSV Log Files

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get csv log files with id | `?id=171` |
| name | Get csv log files matching the name |  `?name=DIWATA1_TLM_200610_123359.qltlm_G2_.csv` |
| satellite | Get csv log files matching the satellite |  `?satellite=Diwata-2` |
| raw_log_file | Get csv log files matching the parent raw log file | `?raw_log_file=ACU__DEC200702_015135_RAW_659x494.rg3` |
| csv_log_type | Get csv log files matching the csv log file type | `?csv_log_type=G1` | 
| date_created | Get csv log files given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| date_uploaded | Get csv log files given date range of uploaded date | `?from:date_uploaded=2020-01-01&to:date_uploaded=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=DIWATA1_TLM_200610_123359.qltlm_G2_.csv&satellite=Diwata-2` |

GET by md5sum

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files/md5sum/<md5sum>`

Get the csv file data in csv/json format:

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files/<id>/csv`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files/<id>/json`


**POST**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/csv_log_files/`

| Parameter | Description | Required |
| ----- | ----- | ----- |
| name | File name for the csv log file |  No. Will get default file name |
| file | The .csv file | Yes |
| raw_log_file | The parent raw log file of the current csv log file | Yes |
| satellite | Diwata-1, Diwata-2, Maya-1 | Yes |
| csv_log_file_type | G1, G2, G3 | No. Will try to get from file name |
| date_created | UTC time when the csv log file was created | Yes |
| uploaded_by | The user who uploaed the file | No |
| md5sum | The md5sum of the raw log file | No. Automatically computed |

**NOTE:** Uploading a csv file will automatically generate logs accordingly. 


### 5. Group 1 Logs

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group1_logs<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group1_logs/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get group 1 log with id | `?id=171` |
| satellite | Get group 1 logs matching the satellite |  `?satellite=Diwata-2` |
| csv_log_file | Get group 1 logs matching the parent csv log file | `?csv_log_file=ACU__DEC200702_015135_RAW_659x494.rg3_dr_rep_G1_.csv` |
| date_created | Get group 1 logs given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| pc_time | Get group 1 logs given date range of pc time | `?from:pc_time=2020-01-01&to:pc_time=2020-01-31` |
| telemetry_time | Get group 1 logs given date range of telemetry time | `?from:telemetry_time=2020-01-01&to:telemetry_time=2020-01-31` |
| epoch_acu_date | Get group 1 logs given date range of epoch acu date | `?from:epoch_acu_date=2020-01-01&to:epoch_acu_date=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-2&from:epoch_acu_date=2020-01-01` |


### 6. Group 2 Logs

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group2_logs<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group2_logs/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get group 2 log with id | `?id=171` |
| satellite | Get group 2 logs matching the satellite |  `?satellite=Diwata-2` |
| csv_log_file | Get group 2 logs matching the parent csv log file | `?csv_log_file=ACU__DEC200702_015135_RAW_659x494.rg3_dr_rep_G2_.csv` |
| date_created | Get group 2 logs given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| pc_time | Get group 2 logs given date range of pc time | `?from:pc_time=2020-01-01&to:pc_time=2020-01-31` |
| telemetry_time | Get group 2 logs given date range of telemetry time | `?from:telemetry_time=2020-01-01&to:telemetry_time=2020-01-31` |
| epoch_acu_date | Get group 2 logs given date range of epoch acu date | `?from:epoch_acu_date=2020-01-01&to:epoch_acu_date=2020-01-31` |
| gps_date | Get group 2 logs given date range of gps date | `?from:gps_date=2020-01-01&to:gps_date=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-2&from:epoch_acu_date=2020-01-01` |


### 7. Group 3 Logs

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group3_logs<parameter/s>`

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/group3_logs/<id>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| id | Get group 3 log with id | `?id=171` |
| satellite | Get group 3 logs matching the satellite |  `?satellite=Diwata-2` |
| csv_log_file | Get group 3 logs matching the parent csv log file | `?csv_log_file=ACU__DEC200702_015135_RAW_659x494.rg3_dr_rep_G3_.csv` |
| date_created | Get group 3 logs given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| pc_time | Get group 3 logs given date range of pc time | `?from:pc_time=2020-01-01&to:pc_time=2020-01-31` |
| telemetry_time | Get group 3 logs given date range of telemetry time | `?from:telemetry_time=2020-01-01&to:telemetry_time=2020-01-31` |
| epoch_acu_date | Get group 3 logs given date range of epoch acu date | `?from:epoch_acu_date=2020-01-01&to:epoch_acu_date=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-2&from:epoch_acu_date=2020-01-01` |

### 8. Stats

#### a. Satellite Temp Min Max

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/satellite_temp_min_max`

#### b. Satellite Power Data

**GET**

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/satellite_power_data<parameter/s>`

Returns satellite power data and other parameters based on depacketed metadata on merged raw images.

| Parameter | Description | Example |
| ----- | ----- | ----- |
| ordering | Order returned viewset based on a parameter. Default ordering is `capture_date` | `?ordering=date_uploaded` |
| satellite | Get logs matching the satellite |  `?satellite=Diwata-2` |
| date_created | Get logs given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| date_uploaded | Get logs given date range of uploaded date | `?from:date_uploaded=2020-01-01&to:date_uploaded=2020-01-31` |
| date_depacketed| Get logs given date range of depacketed date | `?from:date_depacketed=2020-01-01&to:date_depacketed=2020-01-31` |
| capture_time | Get logs given date range of capture time | `?from:capture_time=2020-01-01&to:capture_time=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-2&ordering=date_depacketed` |


#### c. Satellite GPS Data

`https://api.ops.phl-microsat.upd.edu.ph/satellite_health_management/satellite_gps_data<parameter/s>`

Returns satellite GPS position, velocity data over time. Based on group 3 logs.

| Parameter | Description | Example |
| ----- | ----- | ----- |
| ordering | Order returned viewset based on a parameter. Default ordering is `epoch_acu_date` | `?ordering=date_created` |
| satellite | Get logs matching the satellite |  `?satellite=Diwata-2` |
| date_created | Get logs given date range of created date | `?from:date_created=2020-01-01&to:date_created=2020-01-31` |
| epoch_acu_date | Get logs given date range of epoch acu date | `?from:epoch_acu_date=2020-01-01&to:epoch_acu_date=2020-01-31` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?satellite=Diwata-2&ordering=-id` |


## Core Module
### 1. Satellites

`https://api.ops.phl-microsat.upd.edu.ph/core/satellites<parameter/s>`

| Parameter | Description | Example |
| ----- | ----- | ----- |
| name | Get satellites by name | `?name=Diwata` |
| short_name | Get satellites by short_name | `?short_name=D1` |
| avatar_initials | Get satellites by avatar_initials | `?avatar_initials=D1` |
| color | Get satellites by name | `?color=""` |
| status | Get satellites by name | `?status=Available` |
| catalog_no | Get satellite by catalog_no | `?catalog_no=43678` |
| created_time | Display satellites in date range | `?from:created_time=2019-12-01&to:created_time=2019-12-25` |
| *Mixins* | Mix different filters by using **&** to succeeding parameters | `?name=Diwata&status=Available` |
| search | Display data with related keyword | `search=keyword` |
