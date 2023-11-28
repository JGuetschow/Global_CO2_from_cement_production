# Global CO2 from Cement Production Dataset

This repository downloads the Andrew dataset on global CO2 emissions from cement production from Zenodo.

## Description

This repository downloads data on global CO2 emissions from cement production from [Zenodo](https://zenodo.org/records/10008931).
The downloaded dataset can then be converted into CSV (.csv file extension) or NetCDF (.nc file extension) format.
The data management tool [DataLad](http://docs.datalad.org/en/stable/) is used to version control the data sets.
Commands to run the scripts are executed via the pydoit package.

### Installation

- Install datalad according to the [DataLad handbook](https://handbook.datalad.org/en/latest/intro/installation.html). It is recommended to install globally. 
- DataLad is based on Git. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) needs to be installed to run DataLad. 
- Install [Python](https://www.python.org)
- [pydoit](https://pydoit.org/install.html)

## Getting Started

### 1. Clone the repository

Download the repository using the following command.
```
datalad clone
```
Do not use **git clone** to download the repository! This way DataLad will not have the necessary
information to run the program.

### 2. Easy Access
Users who simply want to download the dataset have the option to access both the
original and extracted files with the following command.
```
dataland get <filename>
```
For example, the CSV file for the 2023/09/13 release can be downloaded with:
```
datalad get extracted_data/v230913/Robbie_Andrew_Cement_Production_CO2_230913.csv 
```


### 3. Executing the program

#### 3.1 Set up the virtual environment with doit
```
doit setup_env
```
#### <a name="download"></a> 3.2 Download the version from the command line.
This will download all files from Zenodo as they are.
```
doit download_version --version <YYMMDD>
```
#### <a name="convert"></a> 3.3 Convert the data sets into CSV and NetCDF files.
```
doit read_version --version <YYMMDD>
```


## <a name="newversion"></a> How to add a new version


1. To add a new version go to **versions.py** in the **src** directory and create a new value in the
dictionary. Fill all the required information similar to the previous entries.
For example, the value _v230913_ in the _versions_ dictionary describes the 13-Sep-2023 release.
````python
versions = {
    "v230913": {
        'date': '13-Sep-2023',
        'ver_str_long': 'version 230913',
        'ver_str_short': '230913',
        "folder": "v230913",
        "transpose": False,
        "filename": "0. GCP-CEM.csv",
        'ref': '10.5281/zenodo.8339353',
        'ref2': '10.5194/essd-11-1675-2019',
        'title': 'Global CO2 emissions from cement production',
        'institution': "CICERO - Center for International Climate Research",
        'filter_keep': {},
        'filter_remove': {},
        'contact': "johannes.guetschow@climate-resource.com",
        'comment': ("Published by Robbie Andrew, converted to PRIMAP2 format by "
                    "Johannes Gütschow"),
        'unit': 'kt * CO2 / year',
        'country_code': True,
    },
}
````

2. Then run the two commands as described in [3.2] and [3.3].

## Help
Show all doit commands
```
doit help
```
See a list with possible doit commands specific to this repository
```
doit list
```

Get help on a specific command

```
doit help <command>
```

## Related gin repositories 
_add a reference to our other gin repositories_


## For developers
#### Repository structure
- **.datalad/** contains config file for datalad
- **downloaded_data/** contains original data from Zenodo.
- **extracted_data/** contains data in .csv and .nc format
- **literature/** contains link to publication by Robbie M. Andrew. Can be downloaded with _datalad get_ command
- **src/** 
  - **download_version.py** downloads files from zenodo for a given version. The version to read will be taken from the command line using _argparse_.
  - **download_version_datalad.py** calls datalad to run the data reading function.
  - **helper_functions.py** contains a function to map country codes.
  - **read_version.py** reads the data for a given version and saves to [PRIMAP2](https://primap2.readthedocs.io/en/stable/) native and
    interchange format.
  - **read_version_datalad.py** calls datalad to run the data reading function.
  - **version.py** is a dictionary that contains metadata for each release. This file should be updated when [adding a new version](#a-namenewversiona-how-to-add-a-new-version) 
- **dodo.py** defines pydoit commands.
- **pyproject.toml** configuration file
- **requirements.txt** requirements
- **requirements_dev.txt** development requirements
- **setup.cfg** requirements
- **setup.py** installs python packages
