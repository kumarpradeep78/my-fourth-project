# my-fourth-project
Whenever we are updating anything on our website we have always a fear of downtime(When our Website is not accessible by client).

So today my article is based on how to we can update our website with ZERO DOWNTIME.....

This task is given by VIMAL DAGA in DevOps Assembly Line training.

Task Overview:

Create A dynamic Jenkins cluster and perform this task.

Steps to proceed as:

1. Create a container image that has Linux and other basic configuration required to run Slave for Jenkins. ( example here we require kubectl to be configured )

2. When we launch the job it should automatically start a job on slave based on the label provided for a dynamic approach.

3. Create a job chain of job1 & job2 using the build pipeline plugin in Jenkins

4. Job1: Pull the Github repo automatically when some developers push the repo to Github and perform the following operations as:

1. Create the new image dynamically for the application and copy the application code into that corresponding docker image

2. Push that image to the docker hub (Public repository)

( Github code contain the application code and Dockerfile to create a new image )

5. Job2 ( Should be run on the dynamic slave of Jenkins configured with Kubernetes kubectl command): Launch the application on the top of Kubernetes cluster performing following operations:

1. If launching the first time then create a deployment of the pod using the image created in the previous job. Else if deployment already exists then do a rollout of the existing pod making zero downtime for the user.

2. If Application created the first time, then Expose the application. Else don’t expose it.

Let's Start:

For performing this task we require 3 VM

1 - Where Jenkins is running

2 - Where the docker engine is running

3 - Where k8s(Kubernetes) running

Abstract Idea:

We start Jenkins service from VM1 and then here we configure a cloud service that is linked with VM2 where our docker engine is running. We create an image for dynamic Kubernetes in VM2 which will help us to run our Kubernetes cluster on VM3.

Before proceeding to do this step in your VM2:

Open /usr/lib/systemd/system/docker.service file and add -H tcp://0.0.0.0:3456
(This command will help you to access the docker service of this VM from another machine[this process is called socket binding]).


and then reload and restart your docker services

```javascript
systemctl daemon-reload
systemctl restart docker

Now go to your VM1 and start your Jenkins service and install the "docker" plugin

```javascript
systemctl start jenkins


and then configure your cloud as I showed below:

```javascript
Jenkins->Manage Jenkins -> Manage nodes and clouds-> configure cloud-> add a new cloud-> docker



