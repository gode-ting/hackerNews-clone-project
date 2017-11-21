### Some useful defintions

- Node - an instance of Docker engine (a virtual machine)
- Tasks - the unit of scheduling in Docker
- Services - defined and run on Nodes, and are made up of Tasks
- Swarm - a cluster of Nodes

### What Docker Swarm is and why we need it

Containers are great, but when you get lots of them running, at some point, you need them all working together to solve business problems.  
The problem is, when you have lots of containers running, they need to be managed: you want enough of them running to handle the load, but not so many they are bogging down the machines in the cluster. And well, from time to time, containers are going to crash, and need to be restarted.  
In other words, all those containers need to be managed.

### Why it can help us eliminate bottlenecks

Swarms are useful for distributing services across many machines. This helps to reduce bottlenecks because we make sure we have “backe-ups” in case one service crashes. It also reduces bottlenecks if, for instance, we have a web application that handles hundreds of thousands of requests. Having a single service to handle this many requests could lead to a crash, instead you can scale your application and distribute more resources across the network. 

### docker node ls

![docker node ls](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/docker-node-ls.png)

### docker service ls

![docker service ls](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/docker-service-ls.png) 
