# Jupyter REES Service

REES stands for [Reproducible Execution Environment Specification](https://repo2docker.readthedocs.io/en/latest/specification.html).
It was conceived by the developers of [Binder](https://mybinder.org) but its
utility extends beyond Binder specifically.

This project is still in the design stage. It grew out of discussions at the
[Jupyter Community Workshop for Scientific User Facilities and High-Performance Computing](https://blog.jupyter.org/jupyter-community-workshop-jupyter-for-scientific-user-facilities-and-high-performance-computing-3afa4a990086).

## Goals

* Reduce the learning curve for creating Binder-compatible repositories by
  adding a UI for specifying dependencies and configuration.
* Enable users to specify their whole environment---not just a kernel, but also
  any Jupyter server or front-end extensions they need.
* Enable users to browse Binder-compatible repos created by others and launch
  them on local compute, perhaps with access to persistent storage (i.e., their
  home directory).
* Deploy a builder for Binder-compliant repos that does not require Kubernetes.
  This is important in environments where cloud services aren't a good fit (i.e.
  large locally-stored datasets) and where there are not local resources to
  maintain a Kubernetes cluster.

## Related Work

* This has some similarities to
  [nb_conda](https://github.com/Anaconda-Platform/nb_conda) but it will produce
  environments (could be conda envs, venvs, or containers) that encompass the
  notebook server and frontend, not just a kernel.
* This has some similarities to Code Ocean's UI for building containers, but it
  will operate _outside_ of a running notebook server, and it will be
  open-source.

## Components

1. A JupyterLab extension that lets you select a folder and choose, via command
   or a context menu, "Make this into a repo2docker image."` (For simple
   specifications, ones without ``apt.txt`` or ``Dockerfile``, it might also be
   useful to support "repo2conda" for an option that does not involve
   containers.)

2. A JupyterHub Service that processes requests from (1). This service has
   permission to run docker and has access to storage where it can cache images.

3. A UI for building a [REES](https://repo2docker.readthedocs.io/en/latest/config_files.html#config-files)
   It would have a section for "Add Python requirements," and a section for "Add
   system requirements," etc. This UI would be stand-alone---not a
   JupyterLab/notebook extension---and could be deployed in the same web app as
   (2). It might be written using [Bootstrap](https://getbootstrap.com), as
   JupyterHub itself is. (Since it is _outside_ the notebook server, it would
   work for JupyterLab and classic notebook; it would not be a lab/notebook
   extension.)

4. A gallery of built REES-compliant images, with the ability to browse static
   renderings of the notebooks within them and then to launch them as named
   servers on the Hub, with the option to mount local storage for persistence.

Components 2--4 seem like they could be one web app, deployed as a Hub Service
the same way nbviewer can be. Component 1 would be a very small labextension
that knows where to find the service.
