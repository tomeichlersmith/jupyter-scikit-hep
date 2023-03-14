# hepynb

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
  tomeichlersmith/hepynb:edge
```

### singularity or apptainer
Build the image from DockerHub into a local SIF image file.
```
apptainer build hepynb.sif docker://tomeichlersmith/hepynb:edge
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
  hepynb.sif
```