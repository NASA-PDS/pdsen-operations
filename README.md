# NASA PDS Engineering Node Operations Team
This repo is a repo to track issues intended for the PDS Engineering Node (EN) Operations Team. These issues may include, but are not limited to, PDS4 NSSDCA Deliveries via [PDS Deep Archive](https://nasa-pds.github.io/pds-deep-archive/index.html), data releases, website updates, or other actions where a repo is unknown.

# Support
For help with the PDS Engineering Node, you can either create a ticket in [Github Issues](https://github.com/NASA-PDS/pdsen-operations/issues) or email pds-operator@jpl.nasa.gov for more assistance.

# Installation

## System Requirements

Prior to installing this software, ensure your system meets the following requirements:

* **Python 3**: This software requires Python 3; it will work with 3.6, 3.7, or 3.8. Python 2 will absolutely not work, and indeed Python 2 came to its end of life January 2020. Run python3 

Consult your operating system instructions or system administrator to install the required packages. For those without system administrator access and are feeling anxious, you could try a local (home directory) Python 3 installation using a Miniconda installation.

## Doing the Installation

We will install the operations too using Python [Pip](https://pip.pypa.io/en/stable/), the Python Package Installer. If you have Python on your system, you probably already have Pip; you can run `pip3 --help` to check.

It’s best install the tools virtual environment, so it won’t interfere with—or be interfered by—other packages. To do so:

```
# Clone the repo or do a git pull if it already exists
git clone https://github.com/NASA-PDS/pdsen-operations.git
cd pdsen-operations

# For Linux / Mac
mkdir -p $HOME/.virtualenvs
python3 -m venv $HOME/.virtualenvs/pdsen-ops
source $HOME/.virtualenvs/pdsen-ops/bin/activate
pip3 install -r build/requirements.txt
```

---

# pds-stats.py

The `pds-stats.py` script can be used to get the total download metrics for Github software tools. Here is an example of how to get metrics for the Validate, MILabel, and Transform tools.

For usage information run `bin/pds-stats.py --help`

## Example Usage
 
1. Activate your virtual environment:

```
source $HOME/.virtualenvs/pdsen-ops/bin/activate
```

2. Execute the script:

```
bin/pds-stats.py --github_repos validate mi-label transform --token $GITHUB_TOKEN
```

---

# ldd-corral.py

This utility is used to autonomously generate the [data dictionaries web page](https://pds.nasa.gov/datastandards/dictionaries/index.shtml) for each PDS4 Build.

This software determines all the discipline LDDs to be included with this release, auto-generates the web page, and downloads and stages all the discipline LDDs from the LDD Github repos.

## Config

The [ldd-corral config](https://github.com/NASA-PDS/pdsen-operations/blob/master/conf/ldds/config.yml) can be modified to add additional discipline LDDs to the workflow.

Format:

```
<github-repo-name>:
  name: a title to be used in the output web page that overrides the <name> from the repo IngestLDD
  description: |
    description here
```

## Usage

For latest usage capabilities:
```
 bin/ldds/ldd-corral.py  --help
```

Base usage example (note: GITHUB_TOKEN must be set):
```
source $HOME/.virtualenvs/pdsen-ops/bin/activate
bin/ldds/ldd-corral.py  --pds4_version 1.15.0.0 --token $GITHUB_TOKEN
```

**Default outputs:**
* Web page:  /tmp/ldd-release/dd-summary.html
* Discipline LDDs: /tmp/ldd-release/pds4

---

# LDD Utility Scripts

## prep_for_ldd_release.sh

1. Execute `bin/prep_for_ldd_release.sh` script as follows to create new branches in all Discipline LDD repos:

TBD

2. Go to each Discipline LDD Repo and create Pull Requests for each new branch (branch names like IM_release_1.15.0.0).
  * PR Title: PDS4 IM Release <IM_version>
  * PR Description:
    ```
    ## Summary
    
    PR for testing LDD with new IM release.
    ```
  * PR Labels: `release`

3. If build failed on new branch, contact the LDD Steward to investigate a potential regression test failure or incompatibiliy with the new IM version.

