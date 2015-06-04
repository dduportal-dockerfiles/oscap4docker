# OpenSCAP4Docker Docker image

## Description

That image embed :
* [OpenSCAP](http://www.open-scap.org/page/Main_Page), an open tool for checking Linux vunlerabilities from SCAP datas
* [OpenScap4Docker script](https://github.com/OpenSCAP/container-compliance), a script provided by openscap developers to run against Docker images and containers

The idea is to use Docker's lightweight isolation to have an auto-sufficient image that embed OpenSCAP4Docker and its dependencies, even if it only need bash as dependency...

[![CircleCi Build Status](https://circleci.com/gh/dduportal-dockerfiles/oscap4docker.svg?&style=shield)](https://circleci.com/gh/dduportal-dockerfiles/oscap4docker)

## Usage

From here, just pre-download the image from the registry :

```
$ docker pull dduportal/oscap4docker:1.0.0
```

It is strongly recommended to use tags, even if dduportal/oscap4docker will work as latest tag is implied.

Then you have to choices : running directly your test or build your own, which enable you to embed your tests.

### Inline run

You just have to share your OpenSCAP4Docker' tests directory inside the container and provide it to OpenSCAP4Docker entrypoint as ```docker run``` argument :

```
$ docker run \
	-v /path/to/the/folder/storing/your/tests:/my-tests \
	dduportal/oscap4docker:1.0.0 \
		/my-tests
```

### Build your own testing image

The goal here is to embed to tests in order to version them or share them, and providing the 'all-in-one' box (e.g. OpenSCAP4Docker + deps. + your tests) as a Docker image artefact :


```
$ cat Dockerfile
FROM dduportal/oscap4docker:1.0.0
MAINTAINER <your name>
ADD ./your-tests /app/oscap4docker-tests
RUN yum install -y -q <your dependencies>
CMD ["/app/oscap4docker-tests/"]
$ docker build -t my-tests ./
...
$ docker run -t my-tests
...
```

## Image content and considerations

### Base image

## Image content and considerations

### Base image

Since this image just need bats and little dependencies, we use [Centos Linux 7](https://registry.hub.docker.com/_/centos/) as a base image.

### Already installed package

We embed a set of basic packages :
* bash : It's a OpenSCAP4Docker dependency,
* make : since I use Makefile for building and testing my Docker images,
* curl (and ca-certificates): because the default embeded wget does not handle HTTPS 

## Contributing

Do not hesitate to contribute by forking this repository

Pick at least one :

* Implement tests in ```/tests/bats/```

* Write the Dockerfile

* (Re)Write the documentation corrections


Finnaly, open the Pull Request : CircleCi will automatically build and test for you
