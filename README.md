# Archived
I've chosen to archive this project since my other project [denv](github.com/tomeichlersmith/denv) can achieve the goals of this project
in a cleaner fashion.

Use the latest Python3 container image and install the most recent versions of jupyterlab and scikit-hep to get
the dependencies I want.
```
denv init python:3
denv python3 -m pip install --user --upgrade jupyterlab scikit-hep
denv jupyter lab
```
_Note_: One could also use python's virtual environments within the denv in order to use different versions
of packages with the same base python version.

---

# jupyter-scikit-hep

Use a more recent version of Python with JupyterLab and other HEP-focused Python analysis tools.

### Why?
CentOS7 is stuck on Python 3.6 and I want better Python 
(and all the improvements to the packages I use that come with that).

I've tried building a custom version of Python myself but that 
took a long time and did not work on the first pass. Using images
can make my environment identical across different systems where I
have my notebooks.

## Usage
Putting these packages into the image also makes the image usable
with runners that treat the image as read-only (e.g. apptainer and
singularity).

### docker or podman
The command below will download the image and then run it.
Subsequent runs will keep using the already downloaded image,
so to get updates you will need to use `docker pull`.
```
docker run \
  --rm -it \
  -p 8888:8888 \
  -v $(pwd -P):/home/jovyan \
  ghcr.io/tomeichlersmith/jupyter-scikit-hep
```

### singularity or apptainer
Build the image from DockerHub into a local SIF image file.
```
apptainer build jupyter-scikit-hep.sif docker://ghcr.io/tomeichlersmith/jupyter-scikit-hep
```
**Note**: You may want to change `APPTAINER_CACHEDIR` so that
apptainer doesn't fill up your home directory with layers.

Start JupyterLab from the container like normal defining your
working directory as the home directory of the JupyterLab user
`jovyan`.
```
apptainer run \
  --bind $(pwd -P):/home/jovyan \
  --bind ${TMPDIR:-/tmp/} \
  --home $(pwd -P) \
  --hostname jupyter-scikit-hep.$(uname -n) \
  --env "PS1=${PS1}" \
  jupyter-scikit-hep.sif
```

## Packages
This is a very thin layer on top of the [jupyter/scipy-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-scipy-notebook) (documentation with packages included linked) that simply installs the [`scikit-hep` metapackage](https://github.com/scikit-hep/scikit-hep/). Just to list common packages that you may already use that are included: pandas, awkward, matplotlib, mplhep, hist, pylhe, h5py, scipy, sympy (and others - inspect links for details or give the notebook a try).

### Additional Packages
Since your working directory is mounted to the home directory within the container, you can add more python packages by installing them as a "user".
Open a terminal in Jupyter Lab and enter your `pip` install command.
```
pip install --user <extra-package>
```
Additionally, if you feel like this package would be helpful for the wider community, 
feel free to open an issue in this repository to include it (or see if it can be included in the scikit-hep metapackage).

## Versions
Following [CalVer](calver.org) with just the date as the format. Check the release notes for a specific tag to learn why that release was made and what packages are included.
