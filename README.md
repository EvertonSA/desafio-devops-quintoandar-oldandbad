# hello-devops

_Please read these instructions carefully._

There are 2 applications in this project:

* **hello-python** is a web page that contains a form; when the form is submitted, hello-python enqueues the message on a RabbitMQ queue.

* **hello-node** is a worker that consumes the RabbitMQ queue and stores any message on a MySQL database.

There's also a `create_database.sql` script, to help you prepare the MySQL database.

Each application contains a short README file with more information.

## Problem

**Deploy this entire stack** in a way that _any message entered on the hello-python form is stored on a MySQL database by hello-node_.

* **There are a few bugs in the code**, and you'll need to fix them to solve this exercise; you should not require any specific knowledge of either Python or NodeJS to solve these issues.

* If you need to make any changes to help you debugging (such as adding logs or catching exceptions) we suggest you keep them so we can understand your thought proccess.

* If you have some knowledge of Python or NodeJS development, feel free to implement or suggest _simple_ improvements to the applications to make them production-ready.

## Solutions

We'll accept _any_ of the following types of solution:

* A script using a CLI, SDK, API or library that deploys the stack on a host running a modern Linux distribution _or_ on the AWS cloud.

* A Docker Compose file _or_ another similar container orchestration solution that deploys the stack on a host running a modern Linux distribution _or_ on the AWS cloud.

* A recipe using one or more configuration management tools (e.g. Terraform, Ansible, Chef, Puppet, CloudFormation, Vagrant, Packer, etc.) that deploys the stack on a host running a modern Linux distribution _or_ on the AWS cloud.

**Important:** please **edit this README file** with step-by-step instructions on _how_ to deploy using your solution. Feel free to also include a short paragraph and/or a diagram explaining your solution.

## Expectations

When assessing this exercise, we will take the following points into consideration:

* Whether the solution works or not
* How _easy_ it is to deploy the solution
* How _resilient_ it is (e.g. if the database takes a few more seconds to start than usual, does the system stop working and never recovers?)

Suppose that a _junior_ developer (who has access to most common Linux distributions and an AWS account) will try to run your solution. Would he be able to install all requirements and run it easily? Would he be able to verify that it works? Should any problems arise (e.g. a package is missing), would he be able to identify and fix it?

We don't expect a production-grade solution, but we expect you to show that you'd be able to deploy a production-grade distributed system given enough tools and time.

## Submissions

You should send us a [git patch](https://git-scm.com/docs/git-format-patch) file with your solution. To do so follow these steps:

1.  Clone (do NOT fork) this repository to your machine:

        $ git clone https://github.com/quintoandar/hello-devops.git

2.  Implement your solution and comit your changes locally:

        $ git commit -am "My solution"

3.  Create a patch file for this repository:

        $ git format-patch origin/master --stdout > result.patch

4.  Email us the `result.patch` file.

Please do **not** fork this repository and do **not** publish your solution online!

## Contact

Feel free to reach out if you have any questions! Also, we expect this problem to take some hours at most, but please do get in touch if you need more time to provide a good solution! It is far better than delivering something that does not work :)

## evarakak hello-devops solution

This topic aims to expose changes applyed to the repository files in order to make the applications provided run and to deploy such using a Dockercompose solution.

 

For the pourpose of this test, the described deployment solution is appropriate. In production situations, I would suggest changes on the overall architechture to avoid scalable future issues. Also, in production, I would run Kubernetes instead of Dockercompose solutions. There was a little study from my side on how to convert Dockercompose files to Kubernetes (Kompose and compose-on-kubernetes) but I prefered to provide the MVC only on the recruiters request. Please let me know if providing a Kubernetes solution would be appropriated to prove the answers provided in the recruiter forms.

 

From a microservice architechture perspective, the best would be to build an image for the hello-node app together with the MySQL database. This crete context boundaries needed in such architechtures. A new microservice for handling the Queue System is also appropriate, exposing API's to be consumed by other microservices.

 

## Deployment solution

 

This solution aims to use Dockercompose deployment solution. The first thing to build was the Dockerfile's, responsable for modeling the application images.

 

There are 2 Dockerfile's needed, one for each application (hello-node and hello-python). In total, there are 4 containers, beeing two for the applications, one for the MySQL database and one for the RabbitMQ queue system. The RabbitMQ needs to bootup before the hello-node application. Since we have a Dockercompose solution, the hello-node application fails to start at the beggining of the **docker-compose up** process but is automatically restarted again by docker-compose.

 

## How to Deploy

This solution was built on top of [GCP](https://cloud.google.com/shell/?hl=pt-br) infrastructure. GCP Cloud shell provides a **_FREE_** f1-micro instance running Debian preinstalled with developer tools and has a home directory that **_persists across sessions_**. Since AWS is the desired cloud platform and there is no equivelant to GCP Cloud Shell on AWS, the requirements for deploying this are:

 

- Debian GNU/Linux version 9

- Docker version 18.03

 

After getting the hands-on the patch file provided within this change, perform the following commands:

```bash

cd hello-devops/deployment

docker-compose build

docker-compose up -d # -d flag to release terminal

```

The application is now exposed to port `8080` and can be accessed in [GCP](https://cloud.google.com/shell/?hl=pt-br) by using `Preview port 8080`. If this solution was deployed on AWS machine with an EIP, use the browser to connect to the EIP on port 8080.

 

To enchance developers experience volumes containing source codes are already mounted to the containers. Whenever a change is detected, it is triggered an automatically build and deploy process.

```bash

logs from detected change

```

##### DEBUG info - to get logs from the deployed containers

```bash

docker ps -a # get [containedid]

docker logs [containerid]

```

 

 

## 1.0.1 - 10/01/2019

### Added

- Deployment files (Dockerfile and Dockercompose) + description;

### Changed

- SQL file (create_database.sql) moved to deployment context;

- Fix hello-node logging bug;

- Fix hello-node RabbitMQ url connection string;

- Fix hello-node insert SQL command;

- Fix hello-python missing enqueue method call;
