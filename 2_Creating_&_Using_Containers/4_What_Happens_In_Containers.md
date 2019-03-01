# What's Going On In Containers

## Table of Contents
- [What's Going On In Containers](#whats-going-on-in-containers)
	- [Table of Contents](#table-of-contents)
		- [Commands](#commands)
		- [Example](#example)
	
### Commands
```
docker container top <containerName>			// processes listed for one container
docker container inspect <containerName>		// details of one container config
docker container stats							// performance stats for all containers
* and more :o *
```
### Example
```
docker container run -d --name nginx nginx
docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
docker container ls			// should see two running containers
docker container top mysql		// should list processes running in mysql container
docker container top nginx		// should list processes running in nginx container
docker container inspect mysql		// should display json array of data used to start mysql container
docker container stats			// streams live performance data back to CLI--useful to see RAM, CPU, disk use locally
```