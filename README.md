# Docker Custom Bridge Network – Container Communication Demo

## Overview

This repository demonstrates how to create and use a **custom Docker bridge network**, attach containers to it, and verify **container-to-container communication** using ICMP (`ping`).
The project follows a practical, command-line driven approach suitable for DevOps learning, interviews, and real-world troubleshooting.

---

## Objectives

* Create a custom Docker bridge network with a defined subnet
* Run containers on default and custom networks
* Attach containers to multiple networks
* Validate inter-container connectivity using `ping`
* Inspect Docker network configuration in detail

---

## Prerequisites

* Linux system (Amazon Linux / Ubuntu / EC2)
* Docker installed and running
* Root or sudo privileges
* Basic Docker command knowledge

---

## Steps Performed

### 1. Run an Nginx Container on Default Bridge Network

```bash
docker run -d --name c1 nginx
```

Verify container status:

```bash
docker ps
```

---

### 2. Create a Custom Bridge Network

```bash
docker network create \
  --subnet 10.0.0.0/16 \
  --driver bridge net-1
```

List available networks:

```bash
docker network ls
```

---

### 3. Inspect the Custom Network

```bash
docker inspect net-1
```

Key observations:

* Driver: bridge
* Subnet: 10.0.0.0/16
* Scope: local

---

### 4. Run a Second Container on the Custom Network

```bash
docker run -d --name c2 --network net-1 nginx
```

---

### 5. Connect Existing Container to Custom Network

```bash
docker network connect net-1 c1
```

Now both containers are attached to `net-1`.

---

### 6. Install Ping Utility Inside Container

Enter container shell:

```bash
docker exec -it c1 /bin/bash
```

Update packages and install ping:

```bash
apt update
apt install iputils-ping -y
```

Exit container:

```bash
exit
```

---

### 7. Verify Container-to-Container Connectivity

Ping the second container from the first container:

```bash
docker exec -it c1 ping 172.17.0.3
```

Result:

* Successful ICMP replies
* 0% packet loss
* Confirms containers can communicate over the bridge network

---

## Key Learnings

* Default `bridge` network is isolated and limited
* Custom bridge networks allow controlled container communication
* Containers can be attached to multiple networks
* Docker provides internal DNS and routing for containers on the same network
* `docker inspect` is essential for network debugging

---

## Screenshots

All execution proof and outputs are stored in the `img/` directory.

```
img/
├── create_net1_network.png
├── docker_inspect_net1.png
├── history.png
├── ping_install_in_container.png
└── ping_success.png
```

These screenshots show:

* Network creation
* Network inspection output
* Command execution history
* Ping installation inside container
* Successful ping between containers

---

## Use Cases

* Learning Docker networking fundamentals
* DevOps interview preparation
* Microservices communication testing
* Container troubleshooting practice

---

## Author

Joy
BCA Graduate | DevOps & Cloud Enthusiast

