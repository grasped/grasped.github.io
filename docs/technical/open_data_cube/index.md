# Open Data Cube Technical Guide

## I. Development
This guide presents the process for installing Open Data Cube (ODC) into a local machine and the proper indexing of DIWATA-2 SMI dataset.

The following has been developed and tested in Windows 10 and Debian systems.

### Install Dependencies

The following software are required to run ODC.

1. [Anaconda](https://www.anaconda.com/products/individual)
2. [PostgreSQL](https://www.postgresql.org/download/)
3. [AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/install-windows.html)
4. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

### Clone ODC development repository
Clone the development reposity where document definitions and user guides are found.
```
git clone https://github.com/grasped/odc.git
```
Enter the directory.
```
cd odc
```


### Install Python and packages in Anaconda environment
Use a conda environment to isolate ODC from other software projects.

Add conda-forge to package channels.
```
conda config --append channels conda-forge
```

Create a conda environment named `odc_env` and install python.
```
conda create --name odc_env python=3.8 datacube
```

Activate the `odc_env` conda environment.
```
conda activate odc_env
```

Install other packages needed by ODC.
```
conda install jupyter matplotlib scipy ipyleaflet geopandas scikit-learn-intelex
```

## Create database
If PostgreSQL is fresh installation, a `user` needs to be setup.

Once a user is present e.g. `postgres`, a database should be created after the user.
```
createdb -h localhost -U postgres datacube

password: ******
```

The next step is creating an integration configuration. The following text can be copied and saved to the directory `C:\Users\odc_user\.datacube_integration.conf ` (example):
```
[datacube]
db_hostname: localhost
db_database: datacube
db_username: postgres
db_password: ******
```

Finally, initialize the database.
```
datacube -v system init
```

### Prepare indexing tools
If the data source is coming from AWS S3, some tools are needed to index the data. Starting with `boto` and `awscli`. Install them using:

```
conda install aiobotocore[boto3,awscli]
```
or (if pip)
```
pip install aiobotocore[boto3,awscli]
```
The next package is the ODC tools itself:
```
pip install odc-apps-dc-tools
```

### Add the product definition
The DIWATA-2 SMI dataset product definition can be added using the following. This document describes the dataset which includes projection and measurement properties of the SMI products.
```
datacube product add s3://diwata-missions/Diwata-2/SMI/collection/diwata_2_smi.yaml
```
or
```
datacube product add https://diwata2-odc.s3.us-east-2.amazonaws.com/eo3-product/diwata2_smi_l1c.odc-product.yaml
```

### Add dataset definition
Finally, the dataset documents are metadata for each data. Each one describes the capture time and location of individual band files. They can be recursively added using the following.
```
s3-to-dc --no-sign-request s3://diwata-missions/Diwata-2/SMI/collection/*.odc-metadata.yaml diwata_2_smi
```
or
```
s3-to-dc --no-sign-request https://diwata2-odc.s3.us-east-2.amazonaws.com/eo3/*.odc-metadata.yaml diwata2_smi_l1c

s3-to-dc --no-sign-request s3://diwata2-odc/eo3/*.odc-metadata.yaml diwata2_smi_l1c
```