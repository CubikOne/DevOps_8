# Automation tools

Automation tools for node provisioning.

## Contents

1. [Chapter I](#chapter-i) 
2. [Chapter II](#chapter-ii) \
   2.1. [Remote node configuration via Ansible](#part-1-remote-node-configuration-via-Ansible) \
   2.2. [Service Discovery](#part-2-service-discovery) 

## Chapter I

When deploying an application on some node, whether it is a production or a test bed, it is necessary to prepare the machine itself for the deployment of the application. As you already know, the docker image already contains all the necessary dependencies to run the application, but there are still a number of parameters that require additional configuration or provisioning. First it can be the installation of packages and tools (such as docker itself or git), or it can be an additional configuration, which depends on the node, the migration of files, etc. Exactly to solve such problems, many tools for automating remote machine configuration have been created, one of which is **Ansible**.

In the world of modern web applications it often turns out that a service that is available at one ip address was, for example, moved to another. In this case, the most acute problem is the manual reconfiguration of the communication channels of various application subsystems, allocated into separate services. This problem is solved by **Consul** performing *Service Discovery*, which allows you to automate the process of configuring communication channels.

## Chapter II

The result of the work must be a report with detailed descriptions of the implementation of each of the points with screenshots. The report is prepared as a markdown file in the `src` directory named `REPORT.MD`.

## Part 1. Remote node configuration via Ansible

In this chapter you will perform a remote node configuration to deploy a multiservice application.

**== Task ==**

1) Create three machines using Vagrant - manager, node01, node02. Do not install docker using shell scripts when creating machines on Vagrant! Forward the node01 ports to the local machine to access the undeployed microservice application.

2) Prepare manager as a workstation for remote configuration (help on Ansible is in the materials).
- Go to manager. 
- Check the connection to node01 via ssh on a private network. 
- Generate a ssh key to connect to node01 from the manager (without passphrase). 
- Copy the docker-compose file and the source code of the microservices to the manager. (Use the project from the src folder and the docker-compose file from the previous chapter. Help on ssh is in the materials.)
- Install Ansible on the manager and create ansible folder in which create the inventory file. 
- Use the ping module to check the connection via Ansible. 
- Place the result of the module in a report.

3) Write the first playbook for Ansible that performs apt update, installs docker, docker-compose, copies the compose file from the manager and deploys the microservice application. 

4) Run the prepared tests through postman and make sure that they are all successful. Show the test results in the report.

5) Form three roles: 
 - the application role performs the deployment of a microservice application using docker-compose,
 - apache installs and runs the standard apache server
 - postgres installs and runs postgres, creates a database with an arbitrary table and adds three arbitrary records to it. 
 - Assign the first role to node01 and the second two roles to node02, check the functionality of the microservices application with postman tests, make sure that postgres and the apache server are available. For Apache, a web page should open in the browser. As for PostgreSQL, you need to connect from the local machine and display the contents of the previously created table with data.

6) Place the files created in this part in the `src\ansible01` folder in your personal repository.

## Part 2. Service Discovery

Now let's move on to Service Discovery. In this chapter, you will simulate two remote services, an api and a database, and make a connection between them through Service Discovery using Consul.

**== Task ==**

1) Write two configuration files for consul (information on consul is in the materials):
- consul_server.hcl:
   - set up the agent as a server;
   - specify the interface directed to the internal Vagrant network in advertise_addr
- consul_client.hcl:
   - set up the agent as a client;
   - specify the interface directed to the internal Vagrant network in advertise_addr 


2) Create three machines using Vagrant - consul_server, api и db. 
- Forward port 8082 from the api to the local machine to access the still undeployed api
- Forward port 8500 with the manager to access the ui consul. 

3) Write a playbook for ansible and four roles: 
- install_consul_server, which:
   - works with consul_server;
   - copies consul_server.hcl;
   - installs consul and all necessary dependencies for it;
   - runs consul service
- install_consul_client, which:
   - works with api and db;
   - copies consul_client.hcl;
   - installs consul, envoy all necessary dependencies for consul; 
   - runs consul and consul-envoy services;
- install_db, which:
   - works with db;
   - installs postgres and runs it;
   - creates `hotels_db` database;
- install_hotels_service, which:
   - works with api;
   - copies the service source code
   - installs `openjdk-8-jdk`
   - Creates global environment variables:
      - POSTGRES_HOST="127.0.0.1"
      - POSTGRES_PORT="5432"
      - POSTGRES_DB="hotels_db"
      - POSTGRES_USER="<user name>"
      - POSTGRES_PASSWORD="<user password>"
   - runs the built jar file with the command: `java -jar <path to hotel-service>/hotel-service/target/<jar file name>.jar`

4) Check the functionality of CRUD operations on hotel service. Show the test results in the report.

5) Place the files created in this part in the `src\ansible02` and `src\consul01` folders in your personal repository.

💡 [Click here](https://forms.yandex.ru/cloud/6475b9e02530c22269917318/), **to share feedback on this project with us**. It's anonymous and will help the Product team make your learning better.
