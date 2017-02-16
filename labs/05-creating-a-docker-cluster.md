# Lab 05 - Creating a Docker cluster

> **Dificulty**: Moderate

> **Time**: 30 minutes

> **Tasks**
- [Prerequisite](#Prerequisite)
- [Task 1: Initializing a Docker Swarm cluster](#task-1-initializing-a-docker-swarm-cluster)
- [Task 2: Joining an existing Docker Swarm cluster](#task-2-joining-an-existing-docker-swarm-cluster)
- [Task 3: Verifying the cluster](#task-3-verifying-the-cluster)
- [Task 4: Start the Docker Swarm visualizer](#task-4-start-the-docker-swarm-visualizer)
- [Task 5: Start a service on the Docker Swarm cluster](#task-5-start-a-service-on-the-docker-swarm-cluster)
- [Task 6: Scaling a service](#task-6-scaling-a-service)

## Prerequisite

Before proceeding with this lab, please familiarize yourself first with the topics below:

* [Docker swarm](https://docs.docker.com/engine/reference/glossary/#/swarm)

**NOTE: we will have 2 different clusters, an "odd" one and an "even one**

## Task 1: Initializing a Docker Swarm cluster

The following command should only be run on `node01` and `node02`:

```
docker swarm init --advertise-addr $(hostname -i) 
```

Ignore the output for now, we will not be adding nodes as workers we will be adding them as masters.

To get the token to join a node as a manager issue the following command:

```
docker swarm join-token manager
```

You should get output similar to the output below:

```
    docker swarm join \
    --token SWMTKN-1-5tuqlwyknrjhzsmngih5zbvgedcvzmkvtlh6blnzvsrugjgy7k-enu5pw4t5t6fz5vr6atjtrl0i \
    172.31.16.245:2377
```

Paste this command into the `#general` Slack channel (state if this is for the ODD or EVEN cluster)


## Task 2: Joining an existing Docker Swarm cluster

All odd nodes (node03, node05,...) need to join the cluster initialized by node01.  All even nodes (node04, node06,...) need to join the cluster initialized by node02.

To have your node join the cluster simple run the command that is provided to you by the team running node01/node02:

```
    docker swarm join \
    --token SWMTKN-1-5tuqlwyknrjhzsmngih5zbvgedcvzmkvtlh6blnzvsrugjgy7k-enu5pw4t5t6fz5vr6atjtrl0i \
    172.31.16.245:2377
```

## Task 3: Verifying the cluster

To ensure that the cluster is running as it should you can run the `docker info` and `docker node ls` commands.

```
docker info

<... ...>
Swarm: active
 NodeID: ezh08kvrya4rtfywc3ph0fyzi
 Is Manager: true
 ClusterID: 8gpcrfgexsisag5piy42jfhqw
 Managers: 1
 Nodes: 1
 Orchestration:
  Task History Retention Limit: 5
<... ...> 

docker node ls
ID                           HOSTNAME         STATUS  AVAILABILITY  MANAGER STATUS
5izxh6aatcwgxn4kmy7wtgbbu    ip-172-31-3-241  Ready   Active
ezh08kvrya4rtfywc3ph0fyzi *  ip-172-31-5-47   Ready   Active        Leader
```
## Task 4: Start the Docker Swarm visualizer

Start the Docker Swarm visualizer (on all nodes), keep in mind that the Docker Swarm node only works if your node is a manager node (so for now `node01` and `node02`):

```
docker run -d -p 9000:8080 -v /var/run/docker.sock:/var/run/docker.sock manomarks/visualizer
```

You can visit the Docker Swarm visualizer on:
- http://node01.docker.gluo.io:9000
- http://node02.docker.gluo.io:9000

## Task 5: Start a service on the Docker Swarm cluster

This should also only be done on `node01` and `node02`.

We will now be deploying our container-info container onto the cluster:

```
docker service create --publish 80:80 --replicas 1 --name container-info trescst/container-info
```

You can now surf to any node on the cluster and you will be able to see the container-info "website".  When you check the visualizer you will see that a container was started on one of the nodes of the cluster.

## Task 6: Scaling a service

Now that we got everything up-and-running, the fun begins!  You can easily scale a service using the `docker service scale` command.  After you have scaled your service make sure to check the visualizer.

```
docker service scale container-info=2
docker service scale container-info=5
docker service scale container-info=1
docker service scale container-info=10
```
