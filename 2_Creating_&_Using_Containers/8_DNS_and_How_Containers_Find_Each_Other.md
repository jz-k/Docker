# Docker Networks : DNS and How Containers Find Each Other

## Table of Contents
- [Docker Networks : DNS and How Containers Find Each Other](#docker-networks--dns-and-how-containers-find-each-other)
	- [Table of Contents](#table-of-contents)
		- [Goals](#goals)
		- [Forget About IPs](#forget-about-ips)
			- [DNS Naming](#dns-naming)
			- [DNS Default Names](#dns-default-names)
		- [--link](#link)
		- [A Note on Compose](#a-note-on-compose)
	
### Goals
- Understand how DNS is the key to easy inter-container comms
- See how it works by default with custom networks
- Learn how to use --link to enable DNS on default bridge network

### Forget About IPs
- Static IPs and using IPs for talking to containers is an **anti-pattern**
	* Should be avoided
	* IP addresses may not be the same minute-to-minute in a Docker container
		- Container may go away, fail and be recreated somewhere else, etc.
#### DNS Naming
- Docker uses container name as equivalent to host name for containers talking to each other
- Docker daemon has a built-in DNS server that containers use by default
```
docker container run -p 380:830 --name webhost -d nginx
docker network create my_app_net	// spawns a new virtual network to attach containers to (with default driver of bridge)
docker container run -d --name nginx --network my_app_net nginx	// starts new container, using my_app_net network
docker network inspect my_app_net	// shows network details (including containers using the network)
```
- Because this network is not the default bridge network, it gets a special feature: **automatic DNS resolution** for all containers on the same virtual network
	* Uses container names
	* If a second container were to use the same network, both containers could speak to each other automatically, using their container names
```
docker container run -d --name my_nginx --network my_app_net nginx	// new container created, using same my_app_net network
docker network inspect my_app_net	// details should now include both containers
/* install ping utility */
docker container exec -it nginx bash	// enter bash within nginx container
	apt-get update
	apt-get install iputils-ping
	exit
docker container exec -it my_nginx ping nginx	// DNS resolution "just works"
/* ctrl+c exits */
```
- Swarm servers may "spin up" in new places, so it's useful to take advantage of this automatic DNS server resolution
#### DNS Default Names
- Docker defaults the hostname to the container's name, but aliases can be used
### --link
- Adds a link to another container
- Default bridge network does not have DNS resolution built in by default, but --link can establish links between containers on the bridge network
	* but it may be better to simply create a new network for containers so this doesn't need to happen every time
### A Note on Compose
- **docker-compose** automatically creates new virtual networks whenever an app is "spun up" with it