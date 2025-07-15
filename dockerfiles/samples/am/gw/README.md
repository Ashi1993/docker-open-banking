# Dockerfile for WSO2 Financial Services API Manager Control Plane
This section defines the step-by-step instructions to build an [Alpine](https://hub.docker.com/_/alpine/) Linux based Docker image for WSO2 Financial Services Identity & Access Management Module.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v20.10.10 or above
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) client


## How to build an image and run

##### 1. Checkout this repository into your local machine using the following Git client command.

```
git clone https://github.com/wso2/docker-open-banking.git
```
> The local copy of the `dockerfiles/samples/am/gw` directory will be referred to as `FSAM_DOCKERFILE_HOME` from this point onwards.

##### 2. Build the Docker image.

- Navigate to `<FSAM_DOCKERFILE_HOME>` directory
- Execute `docker build` command as shown below.
    ```
    docker build -t wso2-am:gw4.0.0 .
    ```


## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
