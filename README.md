Docker swarm-example
=========

Getting started
---------------

Go to [Play with Docker](https://www.play-with-docker.com). You need 2 Instances. 

* Step 1 - Init your swarm:
```
docker swarm init --advertise-addr $(hostname -i)
```
Copy the join command (watch out for newlines) output and paste it in the other terminal.

Show members of swarm
Type the below command in the first terminal:
```
docker node ls
```
That last line will show you a list of all the nodes, something like this:
```
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGE
R STATUS
kytp4gq5mrvmdbb0qpifdxeiv *  node1     Ready   Active        Leader
lz1j4d6290j8lityk4w0cxls5    node2     Ready   Active
```
If you try to execute an administrative command in a non-leader node worker, you’ll get an error. Try it here:
```
docker node ls
```
Creating services
The next step is to create a service and list out the services. This creates a single service called web that runs the latest nginx, type the below commands in the first terminal:
```
docker service create -p 80:80 --name web nginx:latest
docker service ls
```
You can check that nginx is running by executing the following command:
```
curl http://localhost:80
```
Scaling up
We will be performing these actions in the first terminal. Next let’s inspect the service:
```
docker service inspect web
```
That’s lots of info! Now, let’s scale the service:
```
docker service scale web=15
```
Docker has spread the 15 services evenly over all of the nodes
```
docker service ps web
```
Updating nodes
You can also drain a particular node, that is remove all services from that node. The services will automatically be rescheduled on other nodes.
```
docker node update --availability drain node2
docker service ps web
```
You can check out the nodes and see that node2 is still active but drained.
```
docker node ls
```
Scaling down
You can also scale down the service
```
docker service scale web=10
```
Lets check our service status
```
docker service ps web
```
Now bring node2 back online and show it’s new availability
```
docker node update --availability active node2
docker node inspect node2 --pretty
```
