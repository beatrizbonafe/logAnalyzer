[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/logchecker.svg)](https://img.shields.io/pypi/pyversions/logchecker)
[![PyPI Latest Release](https://img.shields.io/pypi/v/logchecker.svg)](https://pypi.python.org/pypi/logchecker)
[![License](https://img.shields.io/pypi/l/logchecker.svg)](https://github.com/laimaretto/logChecker/blob/main/LICENSE)

# logChecker #

The idea was born because of the need for a simple tool that could do pre-post check analysis automatically.

## Table of Contents
- [Setup](#setup)
  - [System Libraries](#system-libraries)
  - [Installation](#installation)
- [Usage](#usage)
  - [Configuration Options](#configuration-options)
  - [Templates](#templates)
    - [Filtering Variables of Templates](#filtering-variables-of-templates)
- [Results](#results)
  - [Tabulation of Logs - Assessment](#tabulation-of-logs---assessment)
  - [Comparision of Logs - Pre/Post Check](#comparision-of-logs---pre-and-post-check)

## Setup ##

### System Libraries
These libraries have been tested under Ubuntu 20.04 and 22.04, Windows10, Python 3.8~3.10.

### Installation

Make sure that you have [Python](https://www.python.org/downloads/) and [PIP](https://pip.pypa.io/en/stable/installing/) installed.

> [!NOTE]
> For Windows users, select "Add Python to PATH" during the installation.


For Ubuntu users:
```bash
pip3 install logChecker
```
For Windows users:

```bash
py -m pip install logChecker
```

The source code is hosted on GitHub at https://github.com/laimaretto/logchecker

Installer for the latest released version available at [Python Package Index (PyPI) - LogChecker](https://pypi.org/project/logchecker)



[Go to Table of Contents](#table-of-contents)


## Usage

LogChecker reads the logs stored in folders. These logs are generally obtained by [`taskAutom`](https://github.com/laimaretto/taskAutom). Though not mandatory, `taskAutom` is suggested as a way of obtaining the logs, because these will be stored in a `json` file automatically.

LogChecker organizes the outputs from several `show` commands on different tabs in an Excel file, helping the verification of information. The data parsing is performed using [templates](#templates).

There are two ways of execution: [assesment](#tabulation-of-logs---assessment) or [pre-post comparison](#comparision-of-logs---pre-and-post-check).

### Configuration Options

LogChecker can be configured through CLI as shown below. It is also possible to run `logchecker -h` directly to show this information.

| Parameter  | Description |
| -------------------: | :---------- |
|`-h` | Show all the parameters and exit. |
|`-pre` | Folder with Pre logs. Must end in "/" |
|`-post` | Folder with Post logs. Must end in "/" |
|`-json` | Logs in `json` format: yes or no. Default = yes.|
|`-tf` | Folder where templates reside. Used both for Pre and Post logs. Default = Templates/  |
|`-tf-post` | If set, use this folder of templates for Post logs |
|`-te` | Engine for parsing. Default = textFSM |
|`-ri` | Router ID to be used within the tables in Excel report: name, ip or both. Default = name |
|`-ga` | Generate ATP document in `.docx` format, based on contents of json files from `taskAutom`. Default = no |
|`-ic` | Adds new column (Idx Pre/Post) in changes detected table, when running comparision. Default = no  |
|`-v` | Show version |

### Templates

The parsing templates are looked for, by default, at the folder `Templates/`. logChecker reads the content of the folder to extract the several parsing templates.

If the CLI parameter `-tf` is missing or if a command/log doesn't match any of the available templates, the parsing operation will be done with a general best-effort basic template, which is already defined within the tool.

There are situations in which a comparison of logs must be done when different version of TIMOS have been used. For such cases it's possible to use different template folder: `-tf` to specify the template folder for the `-pre` logs, and `-tf-post` to specify the template folder for the `-post` logs.

> [!NOTE]
> When different TIMOS versions have been used, the variables of the several templates must be the same for the comparison to be succesful.

<!---To find out a set of Templates that can be used, see [`here`](https://github.com/laimaretto/logTemplates)-->
### Filtering Variables of Templates

Filtering of variables is possible. This can be done by simply adding the comments `#filterAction:` as `exclude` or `include-only`, and listing the variables in `#filterColumns:`, within the comment section of the templates at the very top.

When using `#filterAction: exclude`, all the variables listed under `#filterColumns` will be removed from the final generated dataFrame.

Similarly, if using `#filterAction: include-only`, the specified variables in `#filterColumns` are kept, while all the others are removed.


[Go to Table of Contents](#table-of-contents)

## Results

Here are two examples of how to run logChecker.

All the parameters are in [Configuration Options](#configuration-options).


### Tabulation of logs - Assessment

When doing an assesment, logChecker only needs an input folder with the stored logs. This can be done by using the `-pre` parameter.

```bash
logchecker -pre logs_pre/
##### Successfully Loaded Templates from folder Templates\ #####
##### Logs Loaded Successfully from folder logs_pre/ #####

Saving Excel
# 0 sh_srv_srv_using.template
# 1 sh_srv_sdp_using.template
# 2 sh_srv_sap_using.template
# 3 sh_srv_sdp.template
# 4 sh_port.template
# 5 sh_rtr_iface.template

Total running time: 0.45 seconds
```


### Comparision of logs - Pre and post check

To do a comparison check of pre and post logs, an additional folder is needed. This can be done by using the `-post` parameter.

```bash
logchecker -pre logs_pre/ -post logs_post/
##### Successfully Loaded Templates from folder Templates\ #####
##### Successfully Loaded Templates from folder Templates\ #####
##### Logs Loaded Successfully from folder logs_pre/ #####
##### Logs Loaded Successfully from folder logs_post/ #####

Saving Excel
# 0 sh_srv_srv_using.template
# 1 sh_srv_sdp_using.template
# 2 sh_srv_sap_using.template
# 3 sh_srv_sdp.template
# 4 sh_port.template
# 5 sh_rtr_iface.template

Total running time: 0.46 seconds
```

[Go to Table of Contents](#table-of-contents)