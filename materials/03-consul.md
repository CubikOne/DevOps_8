# Consul


Initially, applications had a monolithic structure - i.e., they consisted of one large service. This approach began to lose popularity for many web solutions because of the increasing relevance of the following problems:


1. Such services are difficult to scale. Even if only one module needs to be scaled, in practice it means rescaling the entire application.
2. Little flexibility. The technology stack for each individual module is also strongly tied to the technologies used in other modules of the monolith.
3. Development complexity. Usually, the larger a monolith becomes, the harder it is to make changes to it, because often there are no clear protocols for interaction between the modules of the project, which often leads to the need to change a large number of modules to introduce additional functionality in just one.


In response to these problems, a new idea emerged - to change the architecture from monolithic to microservices. Now each individual entity is managed by a separate service and communicates with the rest of the modules using a clearly defined protocol. In addition, microservices are now free to locate in a cluster, using completely independent stacks. But, as is usually the case, this led to other equally serious problems:


1. Service Discovery. Services need to know exactly where other services are located, which means a complex configuration of modules, highly dependent on the network topology.
2. Safety. Microservers send data to each other over an open network, which means that this data can be intercepted. A new vulnerability is emerging.


Consul is a tool for implementing the Service Discovery pattern, designed primarily to rid microservice architecture applications of manual configuration of communication channels. Now, instead of direct communication between services, they are registered in a separate *registry* before being deployed as part of a microservice application. Each individual microservice accesses only the proxy server *envoy* situated locally for that microservice (that is, it "goes" to its localhost). The proxy server resolves requests of the microservice automatically, based on which registered in the system ip addresses with the services necessary for this microservice, returns *registry*.


Thus, all services are combined into one logical unit - Service Mesh, which has its own data center, one or more consul-servers and many registered in this Service Mesh services. In addition, Service Mesh provides authorization and encryption of connections between services using transport layer security protocol (TLS), and "naked" traffic of services never leaves a particular node.


<img src="misc/images/consul_diagram.png"  width="400">


Consul has a client-server architecture. A consul server is installed on some machine within the network, which will perform Service Discovery in combination with lightweight agent clients located on the registered machines with services and performing proxying.


The performance of Consul is determined by the configuration file specified when you run Consul. The configuration file can be validated before running with the command `consul validate <config file name>`.


Main fields of the configuration file:


`server` - the flag that controls the type of agent (client/server - true/false)


`advertise_addr` - ip address used to announce to other nodes in the cluster


`bind` - ip address/interface that the agent uses on its local machine


`ports` - ports for consul agents (e.g. grpc)
