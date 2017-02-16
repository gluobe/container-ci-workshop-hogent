# Lab 01 - Installing Docker

> **Dificulty**: Easy

> **Time**: 5 minutes

> **Tasks**
> - [Task 1: Installation](#task-1-installation)
- [Task 2: Verification](#task-2-verification)

## Task 1: Installation

In this first lab we will be installing Docker on our system.  The easiest way however is to install the latest version of Docker is by running a script which is provided by Docker, [https://get.docker.com/](https://get.docker.com/).

NOTE: the installation might take a couple of minutes to complete

```
curl -sSL https://get.docker.com/ | sh
```

## Task 2: Verification

To verify that the installation was succesfull, run the following command:

```
docker version
```

You should get output similar to the output below:

```
Client:
 Version:      1.13.1
 API version:  1.26
 Go version:   go1.7.5
 Git commit:   092cba3
 Built:        Wed Feb  8 06:50:14 2017
 OS/Arch:      linux/amd64

Server:
 Version:      1.13.1
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   092cba3
 Built:        Wed Feb  8 06:50:14 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

If Docker is running properly, you should see both `Client` and `Server` version details. In this step, you issued a CLI command which communicated over the local Unix socket on which the daemon is listening. This socket is `/var/run/docker.sock` by default and is owned by `root`.

NOTE: ensure that you have Docker Version 1.12 or higher running, otherwise you might run into issues in later labs.
