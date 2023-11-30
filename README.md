# Global CO2 from Cement Production Dataset

This repository downloads the Andrews dataset on global CO2 emissions from cement production from [Zenodo](https://zenodo.org/records/831454).
The dataset is converted to the PRIMAP2 format and provided in both the csv-based interchange format and the netCDF-based native primap2 format. Several version of the dataset are available within this repository.

## Description

This repository downloads data on global CO2 emissions from cement production from [Zenodo](https://zenodo.org/records/831454).
The downloaded dataset can then be converted into CSV (.csv file extension) or NetCDF (.nc file extension) format. Converted data are available for the following versions:

- | v231016 |[Zenodo](https://zenodo.org/records/10008931) |
- | v230913 |[Zenodo](https://zenodo.org/records/8339353) |
- | v230428 |[Zenodo](https://zenodo.org/records/7875557) |
- | v220915 |[Zenodo](https://zenodo.org/records/7081360) |
- | v220516 |[Zenodo](https://zenodo.org/records/6553090) |

The data management tool [DataLad](http://docs.datalad.org/en/stable/) is used to version control the data sets.
Commands to manage the data are executed via the [pydoit](https://pydoit.org/) package.

## DataLad datasets and how to use them  
  
This repository is a [DataLad](https://www.datalad.org/) dataset. It provides  
fine-grained data access down to the level of individual files, and allows for  
tracking future updates. In order to use this repository for data retrieval,  
[DataLad](https://www.datalad.org/) is required. It is a free and open source  
command line tool, available for all major operating systems, and builds upon  
Git and [git-annex](https://git-annex.branchable.com/) to allow sharing,  
synchronizing, and version controlling collections of large files.  

## Installation

Note that for [simply downloading the dataset,](#1-easy-access) Python and pydoit are not required.

- Install datalad according to the [DataLad handbook](https://handbook.datalad.org/en/latest/intro/installation.html). We recommend installing datalad globally as managing it from within the venv is not something we do for you.
- Install [Python](https://www.python.org)
- Install [pydoit](https://pydoit.org/install.html). Like with DataLad, we recommend installing pydoit globally as managing it from within the venv is not something we do for you.

## Getting Started

### Clone the repository

A DataLad dataset can be `cloned` by running  

```sh
datalad clone
```

Do not use git clone to download the repository! If you use plain git clone, DataLad will not have the necessary
information to manage the dataset. Once the repository is cloned, it is like using a standard light-weight repository on your local machine.  
At this point, the repository contains only small metadata and information on the identity  
of the files in the dataset, but not the actual *content* of the (sometimes large)  
files.  


<h3 id="1-easy-access"> Easy access </h3>
Users who simply want to retrieve the dataset have the option to access both the
original and extracted files with

```sh
dataland get <filename>
```

This command will trigger a download of the files, directories, or subdatasets  
you have specified.

For example, the CSV file for the 13-Sep-2023 release can be downloaded with

```sh
datalad get extracted_data/v230913/Robbie_Andrew_Cement_Production_CO2_230913.csv 
```

### Stay up-to-date  
  
DataLad datasets can be updated. The command `datalad update` will *fetch*  
updates and store them on a different branch to the one you're currently working on (by default  
`remotes/origin/master`). Running  
  
```  
datalad update --merge  
```  
  
will *fetch* available updates and integrate them in one go.  
  
### Find out what has been done  
  
DataLad datasets contain their history in the `git log`.  By running `git  
log` (or a tool that displays Git history) in the dataset or on specific  
files, you can find out what has been done to the dataset or to individual  
files by whom, and when.

<h2 id="2-contributing"> Contributing </h2>

For those who wish to contribute to the repository, below we go through the key commands you will need to use. 

#### Set up the virtual environment with doit

```sh
doit setup_env
```

#### <a name="download"></a>Download the version from the command line.

This will download all files from Zenodo as they are for a specific version (note this version must already be in `versions.py`, if you want to add a new version, see the section on adding a new version below).

```sh
doit download_version version=<vYYMMDD>
```

For example, the following command will download all files from Zenodo for the 16-Sep-2022 release

```sh
doit download_version version=v220916
```

#### <a name="convert"></a> Read the version from the command line.

Reading data refers to the conversion of the downloaded files into CSV and NetCDF format. Similarly to the download
command, the data is read for a specific version with

```sh
doit read_version version=<vYYMMDD>
```

For example, the following command will read the 16-Sep-2022 release

```sh
doit read_version version=v220916
```

## <a name="newversion"></a> How to add a new version

To add a new version go to `src/versions.py` in the `src` directory and create a new value in the
`versions` dictionary. Fill all the required information similar to the previous entries.
For example, the value for key `"v230913"` in the `versions` dictionary describes the 13-Sep-2023 release.

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

Then run the two commands, `read_version` and `download_version` as described in [Contributing](#2-contributing) for your newly added version. 

## Help

Show all doit commands

```sh
doit help
```

See a list with possible doit commands specific to this repository

```sh
doit list
```

Get help on a specific command

```
doit help <command>
```

### Repository structure

- `.datalad/` contains config file for datalad
- `downloaded_data/` contains original data from Zenodo.
- `extracted_data/` contains data in .csv and .nc format
- `literature/` contains link to publication by Robbie M. Andrew. Can be downloaded with _datalad get_ command
- `src/` 
  - `download_version.py` downloads files from zenodo for a given version. The version to read will be taken from the command line using _argparse_.
  - `download_version_datalad.py` calls datalad to run the data reading function.
  - `helper_functions.py` contains a function to map country codes.
  - `read_version.py` reads the data for a given version and saves to [PRIMAP2](https://primap2.readthedocs.io/en/stable/) native and
    interchange format.
  - `read_version_datalad.py` calls datalad to run the data reading function.
  - `version.py` is a dictionary that contains metadata for each release. This file should be updated when [adding a new version](#a-namenewversiona-how-to-add-a-new-version) 
- `dodo.py` defines pydoit commands.
- `pyproject.toml` configuration file
- `requirements.txt` requirements
- `requirements_dev.txt` development requirements
- `setup.cfg` requirements
- `setup.py` installs python packages

### Make sure to correctly set up the DataLad siblings

Git repositories can configure clones of a dataset as remotes in order to fetch, pull, or push from and to them. A `datalad sibling` is the equivalent of a git clone that is configured as a remote. 

**Query information** about about all known siblings with

```sh
datalad siblings
```

**Add a sibling** to allow pushing to github

```sh
datalad siblings add --dataset . --name <name> --url git@github.com:JGuetschow/Global_CO2_from_cement_production.git
```

SSH-access is needed to run this command. Note that `name` can be freely chosen (we tend to just use "github" for GitHub siblings) 

**Push to the github repository**

```
datalad push --to <name>
```

where `name` should match the name you used above
### Issues

There always issues open regarding coding, some of them easy to resolve, some harder.

### Your ideas

Contributing is ouf course not limited to the categories above. If you have ideas for improvements just open an issue or a discussion page to discuss your idea with the community.

### Technical HowTo for contributors

As we have a datalad repository using github and gin, the process of contributing code and data is a bit different from 
pure git repositories. As the data is only stored on gin, the gin repository is the source to start 
from. As gin currently has a problem with forks (the annexed data is not 
forked) we have to use branches for development and, thus, to contribute you
first need to contact the maintainers to get write access to the gin repository.
You have to clone the repository using ssh to be able to push to it. 
For that you first need to store your public ssh key on the gin server 
(settings -> SSH Keys). 

### Instructions for merge requests

Once you have everything set up you can create a new branch branch and work there. 
When you're done, create a pull request to integrate your work into the main 
branch. This should be done first on github to allow for discussions and review (gin servers don't have the same review features). Afterwards the changes 
can be actually merged on gin (so that the annex is merged properly too). 