# Lab 05 - Installing and using docker-compose

> **Dificulty**: Easy

> **Time**: 10 minutes

> **Tasks**
- [Prerequisite](#Prerequisite)
- [Task 1: Installation](#task-1-installation)
- [Task 2: Using docker-compose](#task-2-using-docker-compose)

## Prerequisite

Before proceeding with this lab, please familiarize yourself first with the topics below:

* [docker-compose](https://docs.docker.com/engine/reference/glossary/#compose)

## Task 1: Installation

Installing docker-compose is easy, simply run the following commands:

```
curl -L https://github.com/docker/compose/releases/download/1.10.0/docker-compose-Linux-x86_64 > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

To verify that `docker-compose` is correctly installed run the `docker-compose version` command, you should seem something similar to:

```
docker-compose version 1.10.0, build 4bd6f1a
docker-py version: 2.0.1
CPython version: 2.7.9
OpenSSL version: OpenSSL 1.0.1t  3 May 2016
```

## Task 2: Using docker-compose 

Clone the voting-app repository:

```
cd ~
git clone https://github.com/docker/example-voting-app.git
```

The example-voting-app stack consists of:

1. python application
2. nodejs applciation
3. .NET application
4. Redis
5. PostgreSQL

The entire stack can be started using a very simple command:

```
cd example-voting-app
docker-compose up
```

NOTE: you will see a lot of output, first you will see the different images being dowloaded, images being build and at the end you will see the output of the actual services.

Test the app:
- Voting: http://nodeXY.docker.gluo.io:5000/
- Result: http://nodeXY.docker.gluo.io:5001/


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to install docker-compose and used it to easily run complex setups.


## Cleanup

Please clean up your environment by running the following commands:

```
docker stop $(docker ps -qa)
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -qa)
```
