# Jupyter-notebooks

This repo contains Dockerfiles for building single user images for use in jupyterhub 
(as well as for dask clusters).

### Repo structure:
- [jupyter_codeserverr_proxy](./jupyter_codeserver_proxy) contains relevant python code
to be able to spin up VSCode in jupter environment.
- `*-notebook` contains Dockerfiles to build the container images

### Building 
For convenience, we have a [Makefile](./Makefile) that can be used to perform:

|Command              |Description|
|---------------------|-----------|
|`make build-package` | To build the jupyter_codeserver_proxy as python distributable wheel package |
|`make build-images`  | To build the notebook container images (which needs the above python package) |
|`make build-all`     | To build the python package as well as the container images |
|`make push-images`   | To push the container images to hub.docker.io |
|`make clean`         | To cleanup the dist and build contents of python package (we do not delete container images) |

