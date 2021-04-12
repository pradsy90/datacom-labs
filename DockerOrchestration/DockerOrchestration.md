# Overview 

For this lab, we are going to be continuing into the Docker world. In this lab, we are going to be exploring Docker Orchestration using Docker Swarm. Take a look at the posted slide deck for a better handle on what Docker Swarm and Orchestration is all about. But it's safe to say that Orchestration is about having a manager node and worker nodes that can be assembled into a cluser and services (and tasks) can be deployed in this cluster to help with uptime and redundancy in a system. 

For this lab, you will be deploying three AWS instances -- all with docker on them. There will be one Manager instance and two Worker instances. You'll have to take a bit of care in how you set up these instances. Whatever the case, let's get started and start checking out Container Orchestration. 

First, you are going to need to launch three (3) new AWS instances. 

.AWS Instances
image::1.png[AWS Instances]

{nbsp} +
{nbsp} +

For each of these instances, you are going to have to set up a pretty comprehensive set of ports to be able to demonstrate what we are trying to demonstrate. I would recommend that you first just set up your instances so that you can SSH into them. This is the default instance setting so you won't need to change or add any ports in your security settings. 

Once you have all three machines launched and you can SSH into all three of them, go to each respective security group and from there, we can add a collection of inbound ports. You will have to do this for each of your three instances. 

.Security Tab
image::2.png[Security Tab]

{nbsp} +
{nbsp} +

.Security Group
image::3.png[Security Group]

{nbsp} +
{nbsp} +

These ports ensure that we are able to set up the Docker Swarm. When you are adding ports, make sure to select 'anywhere' as the source. 

.Anywhere
image::4.png[Anywhere]

{nbsp} +
{nbsp} +

(note: in my image, you are seeing the ports listed twice. This is just a residual of AWS. When you add each port (TCP and UDP), you will only need to add one line with the port type, port number, and 'anywhere')

At this point, you should have three instances that are really all the same (they have different names in AWS and different keys but that's really it). We will differentiate them as Manager and two Workers next. 

On all three of your instances, install and start Docker: 

> sudo yum -y install docker 

> sudo service docker start 

Once you have installed and started Docker on each of them, go to the terminal that is the machine that you want to be the manager. First, we are going to create the Docker Swarm and have the other two Worker machines join the swarm. On the Manager terminal type: 

> sudo docker swarm init --advertise-addr (YOUR PUBLIC IPADDRESS FROM AWS)

This will create the Manager node of the Docker swarm. Once you do this, it'll show you a a Docker swarm join command that will look something like: 

- docker swarm join --token (SOME LONG LIST OF STUFF)

Copy this entire docker swarm join --token statement down somewhere. You'll need it to join your two other Worker machines to the swarm. So, let's do this. Go to the terminal window of your two other machines. These are going to be the Worker machines in this orchestrated system. On each machine, type/paste: 

> docker swarm join --token (SOME LONG LIST OF STUFF)

It should tell you in both cases that you have successfully joined the Docker Swarm. Great job!! We now have three machines that are part of a Docker Swarm. One Manager and Two Workers. What we can do now is deploy containers at the manager level and they will be 'picked up' and run by the workers. So let's do that. 

Bring up the terminal of your Manager machine. Type: 

> sudo docker node ls 

This should show you three nodes. One Manager (Leader) and two Workers. You can have many, many more worker nodes but two is fine in our case. 








