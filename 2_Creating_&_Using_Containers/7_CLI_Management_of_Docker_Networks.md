# CLI Management of Virtual Networks

## Table of Contents
- [CLI Management of Virtual Networks](#cli-management-of-virtual-networks)
	- [Table of Contents](#table-of-contents)
		- [Commands](#commands)
			- [Code](#code)
			- [Terminology](#terminology)
		- [Docker Networks -- Security by Default](#docker-networks----security-by-default)
	
### Commands
- **docker network ls** : show networks
- **docker network inspect** : inspect a network
- **docker network create --driver** : create a network
- **docker network connect** : attach a network to a container
- **docker network disconnect** : detach a network from a container
#### Code
```
docker network ls	// lists networks
docker network inspect	<enoughOfNetworkIDToUniquelyIdentify> // details on network in JSON format, including containers attached to network
docker network create my_app_net	// spawns a new virtual network to attach containers to (with default driver of bridge)
docker network ls	// lists networks, including newly created network
docker network create --help	// surfaces network create options
docker container run -d --name nginx --network my_app_net nginx	// starts new container, using my_app_net network
docker network -- help	// surfaces network options
docker network connect <networkID> <containerNameOrID>	// dynamically creates a NIC in a container on an existing virtual network
docker container inspect <containerNameOrID>	// should now list network possessing <networkID> from above (assuming it is the same name or ID used above)
docker network disconnect <networkID> <containerNameOrID>	// dynamically removes a NIC from a container on an existing virtual network
```
#### Terminology
- --network bridge
	* default Docker virtual network, which bridges through the NAT firewall to the physical network host is connected to
- --network host
	* gains performance by skipping virtual networks, but sacrifices security of container model
- --network none
	* removes eth0 and only leaves you with localhost interface in the container
- network driver
	* built-in or 3rd party extensions that give you virtual network features
- NIC
	* network interface controller
### Docker Networks -- Security by Default
- Create apps so frontend and backend exist on same Docker network
- Inter-communication never leaves host
- All externally exposed ports closed by default
- Containers must be manually exposed via -p, which is good default security
- Security can be further buffered with Swarm and Overlay networks