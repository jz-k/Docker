# Installing Docker

## Table of Contents
- [Installing Docker](#installing-docker)
	- [Table of Contents](#table-of-contents)
		- [Docker Editions](#docker-editions)
			- [Three major types of installs:](#three-major-types-of-installs)
				- [Direct](#direct)
				- [Windows/Mac OS](#windowsmac-os)
				- [Cloud](#cloud)
			- [CE vs EE](#ce-vs-ee)
			- [Stable vs Edge](#stable-vs-edge)
		- [Instructions](#instructions)
		- [Docker on Windows](#docker-on-windows)
			- [Win10 Pro/Enterprise](#win10-proenterprise)
			- [Win7/8/Home](#win78home)
			- [Windows Server 2016](#windows-server-2016)
			- [Running Docker for Windows](#running-docker-for-windows)
			- [Running Docker Toolbox for Windows](#running-docker-toolbox-for-windows)
		- [Docker for Mac](#docker-for-mac)
		- [Docker for Linux](#docker-for-linux)
			- [script](#script)
			- [Docker Store](#docker-store)
			- [Don't Use Pre-Installed Set-Ups](#dont-use-pre-installed-set-ups)
			- [Adding User to Docker Group](#adding-user-to-docker-group)
			- [Docker-Compose and Docker Machine (required)](#docker-compose-and-docker-machine-required)
	
### Docker Editions
- Docker is no longer just a "container runtime"
- Docker moves fast--the version matters, and it matters how it's installed
- Docker CE (Community Edition)
	* free and open-source
- Docker EE (Enterprise Edition)
	* paid per node
#### Three major types of installs:
	- Direct
	- Mac/Win
	- Cloud
##### Direct
    - Linux versions are different per distro
    	* don't use the default package!
	- Raspberry Pi
	- Mainframes
	- Windows Server
##### Windows/Mac OS
	- legacy Docker Toolbox
	- Runs a lightweight VM to run containers in
	- Don't use **brew** to install on Mac
##### Cloud
	- Docker for AWS/Azure/Google
#### CE vs EE
- EE offers more support and extra products (management GUI, etc.)
- EE certified on specific platforms
- CE has broader support (more platforms)
#### Stable vs Edge
- 'Edge' means beta in Docker-world
	* released monthly
	* gets new features first, but only supported for a month
- 'Stable' rolls in three months of Edge features
	* released quarterly
	* EE version is supported for longer (1 year)
	
### Instructions
```
Installing Docker: The Fast Way
Section 2, Lecture 10

If you don't already have docker installed, Docker already has some great guides on how to do it. The rest of this Section is about how to setup Docker on your specific OS, but if you already know which OS you want to install on, here's the short-list for downloading it. The videos after this Lecture are walkthroughs of installing Docker, getting the GitHub repo, getting a code editor, and tweaking the command line if you want to. Feel free to skip any and all of this if you have at least docker version 17.06 and like your current setup :)

Installing on Windows 10 (Pro or Enterprise)

This is the best experience on Windows, but due to OS feature requirements, it only works on the Pro and Enterprise editions of Windows 10 (with latest update rollups). You need to install "Docker for Windows" from the Docker Store : https://www.docker.com/docker-windows

With this Edition I recommend using PowerShell for the best CLI experience. See more info in the next few Lectures.

Installing on Windows 7, 8, or 10 Home Edition

Unfortunately, Microsoft's OS features for Docker and Hyper-V don't work in these older versions, and "Windows 10 Home" edition doesn't have Hyper-V, so you'll need to install the Docker Toolbox (https://docs.docker.com/toolbox/overview/), which is a slightly different approach to using Docker with a VirtualBox VM. This means Docker will be running in a Virtual Machine that sits behind the IP of your OS, and uses NAT to access the internet.

NOTE FOR TOOLBOX USERS: For all examples that use http://localhost , you'll need to replace with http://192.168.99.100

Installing on Mac

You'll want to install Docker for Mac (https://www.docker.com/docker-mac), which is great. If you're on an older Mac with less than OSX Yosemite 10.10.3, you'll need to install the Docker Toolbox (https://docs.docker.com/toolbox/overview/) instead.

Installing on Linux

Do *not* use your built in default packages like apt/yum install docker.io  because those packages are old and not the Official Docker-Built packages. 

I prefer to use the Docker's automated script to add their repository and install all dependencies: curl -sSL https://get.docker.com/ | sh  but you can also install in a more manual method by following specific instructions on the Docker Store for your distribution, like this one for Ubuntu (https://store.docker.com/editions/community/docker-ce-server-ubuntu).

What if None Of These Options Work

Maybe you don't have local admin, or maybe your machine doesn't have enough resources. Well the best free option here is to use play-with-docker.com (http://play-with-docker.com/), which will run one or more Docker instances inside your browser, and give you a terminal to use it with. You can actually create multiple machines on it, and even use the URL to share the session with others in a sort of collaborative experience.  I highly recommend you check it out.  Most of the lectures in this course can be used with "PWD", but it's only limitation really is it's time bombed to 4 hours, at which time it'll delete your servers.
```

### Docker on Windows
- Two types of containers
	* Linux
	* Windows
- Linux containers are the default
	* 'containers' generally means Linux containers
- Preferred version is Docker for Windows
	* works only on Pro/Enterprise
- Win7/8/8.1 or Win10 Home use Docker Toolbox
- Windows Server 2016 also supports Windows containers
#### Win10 Pro/Enterprise
- [Docker for Windows](https://store.docker.com)
- More features than just a Linux VM
- Uses Hyper-V with a tiny Linux VM for Linux containers
- PowerShell native
#### Win7/8/Home
- [Docker Toolbox](https://store.docker.com)
- Runs a tiny Linux VM in VirtualBox via
	```
	docker-machine
	```
- Uses a bash shell to make it more like Linux/Mac options
- Does _not_ support Windows containers
#### Windows Server 2016
- Supports native Windows containers
- Docker for Windows run but is not required
	* Only when running on Server for dev/test; **NOT** for prod
- Docker not available for previous Windows Server versions
- Hyper-V can still run Linux VMs (if they can run Docker!) just fine
	* Anywhere you can run Linux, as long as that distro can run Docker, you can run Docker

#### Running Docker for Windows
- Open PowerShell
```
docker version		// can ensure it is installed
```
- Can also run Hyper-V desktop app and see details
- Check shared drives
	* wherever code must be made available, check the drive
- Advanced settings can be adjusted if application is slow for whatever reason
- Can download [cmdr](https://cmder.net/) for ease of use

#### Running Docker Toolbox for Windows
- Make sure VirtualBox installs!
	* Required to run toolbox
- Run Docker Quickstart
	* Should open a virtual box manager, where settings can be changed
		- (settings can also be edited with docker-machine)
```
docker-machine ls
docker version			// may see "can't connect to server"
* if can't connect *
docker-machine env default
* copy last line from @ and onwards *
* paste into terminal *
```
- Tips:
	* Use Docker Quickstart
		- Auto-creates and auto-starts VM
		- Defaults to bash shell
	* Code paths enabled for Bind Mounts work in C:\Users _only_
		- Must be in user profile
	* Bind Mounts work for code (but often not databases)
	* Linux VM can be recreated with docker-machine
		- new machines can also be created with this command

### Docker for Mac
- Docker Toolbox available for legacy OS
- Docker can also run in a Linux VM
- Docker can also also run in a Windows VM
	* Only works with VMWare Fusion
- Don't use homebrew to install!
	* this includes only the CLI tool
		- can be used to contact remote server, but not super helpful for setting up Docker locally
- If on Edge version, can open settings and choose to switch to Stable
- Make sure to Bind Mount (make available) any path that surfaces code which must be available to Docker
- Can adjust Advanced settings to give more resources to VM
	* NOTE that memory allocation is _up to_ the selected amount
- Test that Docker installed
```
docker version
```
- Can install iTerm2 as a replacement for MacOS stock terminal
- To get tab completion for terminal:
```
// from Docker docs
Mac
Install via Homebrew

    1. Install with brew install bash-completion.

    2. After the installation, Brew displays the installation path. Make sure to place the completion script in the path.

    For example, when running this command on Mac 10.13.2, place the completion script in /usr/local/etc/bash_completion.d/.

sudo curl -L https://raw.githubusercontent.com/docker/compose/1.23.2/contrib/completion/bash/docker-compose -o /usr/local/etc/bash_completion.d/docker-compose

```
- Tips:
	* Make sure you enable code paths for Bind Mounts (/Users selected by default)
	* Bind Mounts work best for code, be careful with databases
	* Can run additional nodes with:
		```
		docker-machine create --driver
		```
		- Builds VMs on the fly with Docker built-in
	* [Troubleshooting](https://docs.docker.com/docker-for-mac/troubleshoot/)
- [Teacher's shell styling](https://www.bretfisher.com/shell/)

### Docker for Linux
- Easiest installation process, best native experience
- Bash completion for Docker commands installed automatically
- Installing in a VM, Cloud Instance, etc--all the same process
- May not work for all distros (Amazon Linux, Linode Linux, etc)
- Three main ways to install:
	1. script
	2. Docker store
	3. docker-machine
#### script
- get.docker.com script (gets latest Edge release)
- Something like:
```
curl -sSL https://get.docker.com/ | sh
```
- [Current instructions](https://get.docker.com/)
#### Docker Store
- Docker store has specific instructions for each distro
- Red Hat Enterprise Linus (RHEL) only officially supports Docker EE (paid)
	* CentOS version will work
#### Don't Use Pre-Installed Set-Ups
- Digital Ocean, Linode, etc.
	* Generally use old versions
- Even if using those services, manually install
#### Adding User to Docker Group
```
sudo usermod -aG docker <username_here>
```
- Doesn't work in all distros
	* That's ok, just append 'sudo' to all requests (Docker needs root access)
#### Docker-Compose and Docker Machine (required)
- Check [here](https://github.com/docker/compose/releases) and [here](https://github.com/docker/machine/releases) for latest instructions
	* Will need to be manually updated
