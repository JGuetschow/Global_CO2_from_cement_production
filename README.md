# Global CO2 from Cement Production Dataset

This repository downloads the Andrew dataset on global CO2 emissions from cement production from [Zenodo](https://zenodo.org/records/10008931).

## Description

This repository downloads data on global CO2 emissions from cement production from [Zenodo](https://zenodo.org/records/10008931).
The downloaded dataset can then be converted into CSV (.csv file extension) or NetCDF (.nc file extension) format.
The data management tool [DataLad](http://docs.datalad.org/en/stable/) is used to version control the data sets.
Commands to run the scripts are executed via the pydoit package.

## DataLad datasets and how to use them  
  
This repository is a [DataLad](https://www.datalad.org/) dataset. It provides  
fine-grained data access down to the level of individual files, and allows for  
tracking future updates. In order to use this repository for data retrieval,  
[DataLad](https://www.datalad.org/) is required. It is a free and open source  
command line tool, available for all major operating systems, and builds up on  
Git and [git-annex](https://git-annex.branchable.com/) to allow sharing,  
synchronizing, and version controlling collections of large files.  

## Installation

- Install datalad according to the [DataLad handbook](https://handbook.datalad.org/en/latest/intro/installation.html). It is recommended to install globally. 
- DataLad is based on Git. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) needs to be installed to run DataLad. 
- Install [Python](https://www.python.org)
- [pydoit](https://pydoit.org/install.html)

## Getting Started

### Clone the repository

A DataLad dataset can be `cloned` by running  
```
datalad clone
```
Do not use **git clone** to download the repository! This way DataLad will not have the necessary
information to run the program. Once a dataset is cloned, it is a light-weight directory on your local machine.  
At this point, it contains only small metadata and information on the identity  
of the files in the dataset, but not actual *content* of the (sometimes large)  
data files.  


### Easy Access
Users who simply want to retrieve the dataset have the option to access both the
original and extracted files with
```
dataland get <filename>
```
This command will trigger a download of the files, directories, or subdatasets  
you have specified.

For example, the CSV file for the 2023/09/13 release can be downloaded with
```
datalad get extracted_data/v230913/Robbie_Andrew_Cement_Production_CO2_230913.csv 
```
### Stay up-to-date  
  
DataLad datasets can be updated. The command `datalad update` will *fetch*  
updates and store them on a different branch (by default  
`remotes/origin/master`). Running  
  
```  
datalad update --merge  
```  
  
will *pull* available updates and integrate them in one go.  
  
### Find out what has been done  
  
DataLad datasets contain their history in the ``git log``.  By running ``git  
log`` (or a tool that displays Git history) in the dataset or on specific  
files, you can find out what has been done to the dataset or to individual  
files by whom, and when.

## Executing the program

#### Set up the virtual environment with doit
```
doit setup_env
```
#### <a name="download"></a>Download the version from the command line.
This will download all files from Zenodo as they are.
```
doit download_version --version <YYMMDD>
```
#### <a name="convert"></a>Convert the data sets into CSV and NetCDF files.
```
doit read_version --version <YYMMDD>
```

## <a name="newversion"></a> How to add a new version


To add a new version go to **versions.py** in the **src** directory and create a new value in the
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
Then run the two commands `read_version` and `download_version` as described in **Executing the program**. 

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

## Contributing
### Repository structure
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

### Make sure to correctly set up the DataLad siblings
Git repositories can configure clones of a dataset as _remotes_ in order to fetch, pull, or push from and to them. A `datalad sibling` is the equivalent of a git clone that is configured as a remote. 

**Query information** about about all known siblings with
```
datalad siblings
```

**Add a sibling** to allow pushing to github
```
datalad siblings add --dataset . --name <name> --url git@github.com:JGuetschow/Global_CO2_from_cement_production.git
```
SSH-access is needed to run this command. Note that _name_ can be freely chosen. 

**Push to the github repository**
```
datalad push --to <name>

```

### Issues
There always issues open regarding coding, some of them easy to resolve, some harder.

### Your ideas
Contributing is ouf course not limited to the categories above. I you have ideas for improvements just open an issue or a discussion page to discuss you idea with the community.

### Technical HowTo for contributors
As we have a datalad repository using github and gin the process of contributing code and data is a bit different from 
pure git repositories. As the data is only stored on gin, the gin repository is the source to start 
from. As gin currently has a problem with forks (the annexed data is not 
forked) we have to use branches for development and, thus, to contribute you
first need to contact the maintainers to get write access to the repository.
You have to clone the repository using ssh to be able to push to it. 
For that you first need to store your public ssh key on the gin server 
(settings -> SSH Keys). 

### Instructions for merge requests
Once you have everything set up you can create a new branch branch and work there. 
When you're done create a pull request to integrate your work into the main 
branch. This should be done first github to allow for discussions and review. Afterwards the changes 
can be implemented on gin. 