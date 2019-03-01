# Getting a Shell Inside Containers

## Table of Contents
- [Getting a Shell Inside Containers](#getting-a-shell-inside-containers)
	- [Table of Contents](#table-of-contents)
		- [Commands](#commands)
		- [No Need to SSH Into Containers](#no-need-to-ssh-into-containers)
		- [Alpine Linux](#alpine-linux)
		- [Recap](#recap)
	
### Commands
```
docker container run -it	// start a new container interactively
docker container exec -it	// run additional command in existing container
```
### No Need to SSH Into Containers
```
docker container run -it	// 'it' is actually two separate options:
					// 't' -- pseudo-tty, simulates a real terminal like what SSH does
					// 'i' -- keep session open to receive terminal input
docker container run -it --name proxy nginx bash	// when 'bash' run with 'it', a new terminal appears with a bash shell running inside the container (will be root in container, but not necessarily root on host machine)
ls -al		// full listing of everything in root of container
		// from here, can download packages, add/delete folders and files, etc--can do anything in the container that can be done in a normal shell!
exit			// exits container
docker container ls		// does not list proxy (the name we gave our terminal process), since we exited
docker container ls -a	// lists proxy
	// note the COMMAND is not the nginx container (the default COMMAND for a container is the program itself)--it is 'bash', which we manually set so we could access the shell
* containers only run as long as the COMMAND run at their startup runs, which is why the bash container we ran exited when bash exited--'bash' was the startup command *
docker container stop nginx
docker container rm nginx
docker container run -it --name ubuntu ubuntu			// default CMD is bash--we don't need to specify
* once installed, can immediately use apt package manager *
apt-get update
apt-get install -y curl		// curl installs, can use just as it would be used on a local machine
curl google.com
exit		// stops container (since 'bash' was the CMD to start)
* if this container were restarted, it would have curl installed, but if a new ubuntu container is run, it WOULD NOT have curl installed--it would be distinct *
docker container start --ai ubuntu		// gets us right back into our previous ubuntu container
exit		// stops container again
// what about running a terminal in a container that is already running a different process? docker container exec
docker container run --publish 3306:3306 --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -d mysql
docker container exec -it mysql bash
* now in the container running mysql, in a bash terminal *
apt-get update && apt-get install -y procps						// install command 'ps' to show running processes
ps aux		// shows all running processes within container
exit
docker container ls			// show mysql still running--did not stop, since its base CMD was not bash, bash was simply a process within the container
docker container stop mysql
docker container rm mysql
```
### Alpine Linux
- Very small, security-focused Linux distribution
```
docker pull alpine		// pulls alpine linux into local image cache
docker image ls			// lists all images in local cache (should now list alpine)
docker container run -it alpine bash		// can't run--bash executable file does not exist in the container!
docker container run -it alpine sh			// sh, however, is included--so this works
* alpine's package manager is apk--can be used to install bash, if needed *
```
### Recap
- **docker container run -it** : starts a new container immediately
- **docker container exec -it** : runs new command in existing container
- **Alpine Linux** (and others): can run shell, alpine in particular ideal for container images