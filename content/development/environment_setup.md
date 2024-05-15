+++
title = "Environment Setup"
insert_anchor_links = "right"
+++

# Environment Setup

The tool was designed to be developed fully on local and owned FOSS infrastructure, however, it is possible to use any more convenient services by porting configurations.

To setup a fully owned local development environment to work on the tool, one needs:

- local development dependencies
    + go
    + gopls
    + delve
    + go-tools
- a hypervisor
    + a debian 12 VirtualBox VM
- a docker registry
- a git host with support for CI/CD
    + forgejo
- a CI/CD platform
    + DroneCI with docker runner

Local development dependencies can be all installed at once through `nix` by running `nix shell` on the repository's root directory.

## 0 Create the Virtual Machine

With the following `Vagrantfile`:

```Vagrantfile
Vagrant.configure("2") do |config|
    config.vm.box = "debian/bookworm64"

    config.vm.hostname = "vagrant-deb-devprivops"
    config.vm.network "private_network", ip: "192.168.56.1"
    config.vm.network "forwarded_port", guest: 8000, host: 8000  # ForgeJo
    config.vm.network "forwarded_port", guest: 8022, host: 8022  # ForgeJo ssh
    config.vm.network "forwarded_port", guest: 8080, host: 8080  # Drone CI
    config.vm.network "forwarded_port", guest: 5000, host: 5000  # Docker Registry

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.memory = "16000"
        vb.cpus = 6
        vb.name = "vagrant-deb-devprivops"
        check_guest_additions = false

        # make internet work https://serverfault.com/a/496612
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        #vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end
end
```

Run `vagrant up`

## 1 Install Docker

Run `vagrant ssh` to enter the VM

```sh
# https://docs.docker.com/engine/install/debian/
# Add Docker's official GPG key:
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y ca-certificates curl
sudo install -y -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update -y

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo addgroup docker
sudo adduser vagrant docker
```

Log out and back in with `Ctrl + D` and `vagrant ssh`

## 2 Install Forgejo

Use the following `docker-compose.yml`

```yml
# From https://forgejo.org/docs/latest/admin/installation-docker/
version: '3'

networks:
  forgejo:
    external: false

services:
  server:
    image: codeberg.org/forgejo/forgejo:1.21
    container_name: forgejo
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - FORGEJO__webhook__ALLOWED_HOST_LIST=192.168.56.1
    restart: always
    networks:
      - forgejo
    volumes:
      - ./forgejo:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - '3000:3000'
      - '222:22'
```

Then `docker compose up -d`

After that, from the host, acessing `http://192.168.56.1:8000` will prompt for installation tasks to be done in the browser.
In the end, it's just clicking `Install Forgejo`.

## 3 Install DroneCI

Follow instructions as per https://docs.drone.io/server/provider/gitea/

```sh
docker run \
	--volume=/var/lib/drone:/data \
	--env=DRONE_GITEA_SERVER=http://192.168.56.1:8000 \
	--env=DRONE_GITEA_CLIENT_ID=f6e729b8-24ee-427a-9c7c-76cf9c52d67c \
	--env=DRONE_GITEA_CLIENT_SECRET=gto_qg62xdvrz54t45n2gc2irddfcujho2rbkctupqunivszbbg4mclq \
	--env=DRONE_RPC_SECRET=7ceed18af2b05f94977c88f3d4f48550 \
	--env=DRONE_SERVER_HOST=192.168.56.1:8080 \
	--env=DRONE_SERVER_PROTO=http \
	--publish=8080:80 \
	--publish=443:443 \
	--restart=always \
	--detach=true \
	--name=drone \
	drone/drone:2
```

### 3.1 Install runners

Follow instructions as per https://docs.drone.io/runner/docker/installation/linux/ 

```sh
docker run --detach \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_RPC_PROTO=http \
  --env=DRONE_RPC_HOST=192.168.56.1:8080 \
  --env=DRONE_RPC_SECRET=7ceed18af2b05f94977c88f3d4f48550 \
  --env=DRONE_RUNNER_CAPACITY=1 \
  --env=DRONE_RUNNER_NAME=docker-runner \
  --publish=3000:3000 \
  --restart=always \
  --name=runner \
  drone/drone-runner-docker:1
```

## 4 Install Docker Registry

```sh
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

Allow insecure registry by adding the following to `/etc/docker/daemon.json`

```json
{ 
    "insecure-registries": ["host:port"] 
}
```

### 4.1 Build golang-fuseki image

```Dockerfile
# Use official Golang image
FROM golang:latest

# Install necessary dependencies
RUN apt update -y && apt install -y \
    wget \
    unzip \
	default-jdk

# Download and install Apache Jena Fuseki
WORKDIR /opt
RUN wget https://dlcdn.apache.org/jena/binaries/apache-jena-fuseki-5.0.0.tar.gz && \
    tar xzf apache-jena-fuseki-5.0.0.tar.gz && \
    rm apache-jena-fuseki-5.0.0.tar.gz && \
    mv apache-jena-fuseki-5.0.0 fuseki

# Expose the Fuseki port
EXPOSE 3030

# Start Fuseki server
CMD ["/opt/fuseki/fuseki-server", "--mem", "--port=3030", "/tmp"]
```

```sh
docker build -t golang-fuseki:latest .
docker tag golang-fuseki:latest 192.168.56.1:5000/golang-fuseki:latest
docker push 192.168.56.1:5000/golang-fuseki:latest
```

