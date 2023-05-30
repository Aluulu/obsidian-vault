Docker is an open-source deployment application that utilises containers to easily deploy software on a variety of systems. Docker does this by providing a consistent environment for applications to run in, as well as their dependencies. A application installed by Docker should "just work".

# Why use Docker

- **Consistent and reproducible environments** - Docker containers are consistent. The container provides a consistent and isolated environment that can be reproduced across different machines, ensuring that your application behaves the same way everywhere.
- **Portability** - Since Docker containers are exactly the same, they can be used on any machine regardless of it's settings.
- **Efficient** - Multiple containers can be ran on the same machine, sharing the host's resources. Unlike Virtual Machines, containers can be ran on the bare metal operating system, eliminating potential overhead.
- **Isolation and security**: Each Docker container runs in its own isolated environment, with its own file system, processes, and network interfaces. This isolation provides an added layer of security by limiting the impact of potential vulnerabilities. Containers also enable you to separate different components of your application, reducing the risk of conflicts.
- **Scalability and flexibility**: Docker makes it easy to scale your application horizontally by spinning up multiple instances of containers. You can quickly add or remove containers based on demand

# Installation

## Ubuntu

>[!Important]
>Make sure Docker is uninstalled by your package manager or other means before installing it as they conflict each other. Run the following command to uninstall Docker using apt:
>```Shell
># Uninstall using the Ubuntu package manager
>for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
>```
>```Shell
># Remove Images, containers, volumes, and custom config files
>sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
>```


### 1. Set up the repository

Install the packages that allow use of a repository over HTTPS

```Shell
 sudo apt-get install ca-certificates curl gnupg
```

### 2. Add Docker's official GPG key

```Shell
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 3. Add the Docker repository to the sources list

Add the docker repository so that you can download Docker from the official repository

```Shell
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Update the package list

Run `apt update` to update the package list to include the new Docker repository

```Shell
sudo apt-get update
```

### 5. Install the latest version of Docker

Download and install Docker from the official repository

```Shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 6. Verify installation

Verify the installation by running the following command. You should see a confirmation message informing that Docker has been set up correctly.

```Shell
sudo docker run hello-world
```

### 7. (Optional) Manage Docker as a non-root user

Docker creates a group called Docker when installed, this group allows you to run Docker commands as a non-root user. To do this, use the following command:

>[!Important]
>Doing this will give the user root privileges, so be careful and ensure the user should be allowed these privileges before continuing.
>
>For more information regarding the risks, please use the official docs guide: https://docs.docker.com/engine/security/#docker-daemon-attack-surface

```Shell
sudo usermod -aG docker $USER
```

## Red Hat Enterprise Linux

>[!Important]
>Make sure Docker is uninstalled by your package manager or other means before installing it as they conflict each other. Run the following command to uninstall Docker using apt:
>```Shell
># Uninstall using the RHEL package manager
>sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine podman runc
>```
>```Shell
># Remove Images, containers, volumes, and custom config files
>sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
>```

### 1. Set up the repository

```Shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```

### 2. Install the latest version of Docker

```Shell
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 3. Start Docker

```Shell
sudo systemctl start docker
```

### 4. Verify installation

```Shell
sudo docker run hello-world
```

### 5. (Optional) Manage Docker as a non-root user

Docker creates a group called Docker when installed, this group allows you to run Docker commands as a non-root user. To do this, use the following command:

>[!Important]
>Doing this will give the user root privileges, so be careful and ensure the user should be allowed these privileges before continuing.
>
>For more information regarding the risks, please use the official docs guide: https://docs.docker.com/engine/security/#docker-daemon-attack-surface

```Shell
sudo usermod -aG docker $USER
```

# Common commands

### Check running containers

```Shell
docker ps
```


# Docker volumes

Docker volumes are a feature in Docker that enable persistent storage for containers. By default, when a container is created, it runs in an isolated environment and any data written to its file system is temporary. When a container is stopped or removed, the data within it is lost.

However, Docker volumes provide a way to store and manage data outside the container's file system, allowing it to persist even when the container is stopped or deleted. Volumes can also be shared and reused among multiple containers.

## How to create a volume

To create a volume, simply use the following command:

```Shell
docker volume create [volume-name]
```

## List volumes

To list the available volumes, simply use the following command:

```Shell
docker volume ls
```

## Inspect volume

To view detailed information about a specific volume, you can use the following command:

```Shell
docker volume inspect [volume-name]
```

## Remove volume

To remove a volume, you can use this command:

```Shell
docker volume rm [volume-name]
```

## Attach a volume to a container

>[!Important]
>You cannot add a volume to a running container. The container must be stopped before a volume can be attached.

### Docker compose file

The easiest way to attach a volume to a container is to use the `docker compose file`, and adding it there. You can do this by adding the following line underneath the `volumes:` header:

```yml
- path/to/directory:/volume-name
```

#### Example

```yml
    volumes:
      - /path/to/jellyfin-config:/config
      - /path/to/jellyfin-cache:/cache
      - /path/to/jellyfin-media:/media
```

### Docker run

You can also use `docker run` to add a container. Use the following command to do so:

```Shell
docker run --mount source=[volume-name],target=[/path/to/data] [image-name]
```

#### Example

```Shell
docker run --mount source=mydata,target=/app/data nginx:latest
```

# Docker networks

Docker containers each get their own isolated network by default. This helps prevent your container from reaching other networks or machines, even within the same network. Docker allows you to create virtual networks that can communicate with other containers and even other networks.

There are 5 different types of Docker networks, these are:

- **bridge**: The default network driver. If you don’t specify a driver, this is the type of network that is created. Bridge networks allows your container to connect to your network, but not to other containers. See [Use bridge networks](https://docs.docker.com/network/bridge/) on the official Docker docs.
- **host**: Used standalone containers, this network remove network isolation between the container and the Docker host, and use the host’s networking directly. The container will use the host's IP address and ports for the container's use. See [Use host networking](https://docs.docker.com/network/host/) on the official Docker docs.
- **overlay**: Overlay networks connects multiple Docker containers together and enable swarm services to communicate with each other. You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons. This network type removes the need to do OS-level routing between these containers. See [Use overlay networks](https://docs.docker.com/network/overlay/) on the official Docker docs.
- **ipvlan**: IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration. See [Use IPvlan networks](https://docs.docker.com/network/ipvlan/) on the official Docker docs.
- **macvlan**: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the `macvlan` driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack. See [Use macvlan networks](https://docs.docker.com/network/macvlan/) on the official Docker docs.
- **none**: For this container, disable all networking. Usually used in conjunction with a custom network driver. `none` is not available for swarm services. See [Disable networking for a container](https://docs.docker.com/network/none/) on the official Docker docs.

## How to select a network

To select a network, simply edit the Docker compose file to include the relevant network mode. 

```yml
network_mode: '[network_type]'
```

### Example

```yml
services:  
	jellyfin:  
		image: jellyfin/jellyfin  
		container_name: jellyfin  
		network_mode: 'host'
```