================
Quickstart Guide
================

Installation
==============

The benchmarks do not need to be installed. Each benchmark is provided as a Docker image hosted on dockerhub and can be run with a single command, which you can find on the respective benchmark's documentation page.

Requirements:

* Docker (available for all major operating systems at www.docker.com)

Running a first model
========================

We begin by starting the tsunami model from the benchmark library. To make it more interesting, we also mount the output directory specified in the model's documentation to a folder in our home directory. This will allow us later to conveniently access output files from the model.

``docker run -it -p 4242:4242 -v ~/tsunami_output:/output linusseelinger/model-exahype-tsunami``

One way to trigger a run is to use the ``umbridge-client.py`` example for UM-Bridge. You can find the script at www.github.com/UM-Bridge/umbridge/tree/main/clients/python.

``python3 umbridge-client.py http://localhost:4242``

This example triggers two runs of the model. The second one activates the model's VTK output. You can access the resulting files in the folder we mounted above in your home directory, and view them using a suitable tool like ParaView.

Alternatively, any other UM-Bridge client can connect to the model. You can even do a raw HTTP request using the `curl` command line tool.

``curl http://localhost:4242/Evaluate -X POST -d '{"input": [[100.0, 50.0]], "config":{"vtk_output": true}}'``

Contributing
==============

We welcome any new benchmarks added to the collection. To add a new benchmark you will need a Dockerfile defining the setup of your software and a README.md. Place these into a folder within benchmarks. You can access all existing benchmarks definitions in this repository for reference.

Dockerfile
------------

The Dockerfile defining how your model or benchmark Docker image should be built is fairly simple. You can start from any base image (e.g. Debian Linux), proceed with instructions for installing possible dependencies, copy in your code and (if needed) compile it.

The only assumption on the resulting image is that, when a container is spun up, your application supporting the UM-Bridge interface is run (this can be achieved with an appropriate RUN command).

README
------------

The README is automatically incorporated in this documentation. It must follow the structure prescribed by the template README located in the docs folder.
Ensure that all appropriate sections are filled.

Some components of the README can be auto-generated by using the ``generate_benchmark_readme_info.py`` in the docs folder. This script connects to your running model server and extracts basic information from the UM-Bridge protocol.

To include images in the readme either link to an external website or place the image in your benchmark's directory and provide the full path to the raw image.
