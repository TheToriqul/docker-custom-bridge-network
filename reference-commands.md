# Docker Custom Bridge Network Reference Commands

- [Section 1: Core Network Operations](#section-1-core-network-operations)
  - [Step 1: Create a Custom Bridge Network](#step-1-create-a-custom-bridge-network)
  - [Step 2: List Docker Networks](#step-2-list-docker-networks)
  - [Step 3: Launch Containers Connected to the Network](#step-3-launch-containers-connected-to-the-network)
  - [Step 4: Verify Container Status](#step-4-verify-container-status)
  - [Step 5: Verify Communication Between Containers](#step-5-verify-communication-between-containers)
  - [Step 6: Cleanup](#step-6-cleanup)
- [Section 2: Advanced Network Management](#section-2-advanced-network-management)
  - [Connect an Existing Container to a Network](#connect-an-existing-container-to-a-network)
  - [Disconnect a Container from a Network](#disconnect-a-container-from-a-network)
  - [Create a Network with Custom Subnet and Gateway](#create-a-network-with-custom-subnet-and-gateway)
  - [Assign Static IP to a Container](#assign-static-ip-to-a-container)
- [Section 3: Network Troubleshooting](#section-3-network-troubleshooting)
  - [Inspect Network Details](#inspect-network-details)
  - [Examine Container Network Settings](#examine-container-network-settings)
  - [Test Connectivity with Ping](#test-connectivity-with-ping)
  - [Use Netshoot for Advanced Troubleshooting](#use-netshoot-for-advanced-troubleshooting)

> **Author**: [Md Toriqul Islam](https://linkedin.com/TheToriqul)  
> **Description**: Reference commands for creating and managing custom bridge networks in Docker  
> **Learning Focus**: Mastering Docker networking concepts and configurations  
> **Note**: This is a non-executable documentation file. Do not execute commands directly without understanding their implications.

## Section 1: Core Network Operations

### Step 1: Create a Custom Bridge Network
```bash
# Create a custom bridge network named 'my-bridge-network'
docker network create --driver bridge my-bridge-network

# Inspect the network details
docker network inspect my-bridge-network
```

### Step 2: List Docker Networks
```bash
# List all existing Docker networks
docker network ls
```

### Step 3: Launch Containers Connected to the Network
```bash
# Launch container1 and connect it to 'my-bridge-network'
docker run -d --name container1 --network=my-bridge-network nginx

# Launch container2 and connect it to 'my-bridge-network'
docker run -d --name container2 --network=my-bridge-network nginx

# Launch container3 and connect it to 'my-bridge-network'
docker run -d --name container3 --network=my-bridge-network nginx
```

### Step 4: Verify Container Status
```bash
# List running containers
docker ps
```

### Step 5: Verify Communication Between Containers
```bash
# Access the shell of container1
docker exec -it container1 /bin/bash

# Ping container2 from container1
ping container2 -c 5

# Ping container3 from container1
ping container3 -c 5

# Access the shell of container2
docker exec -it container2 /bin/bash

# Ping container1 from container2
ping container1 -c 5

# Ping container3 from container2
ping container3 -c 5
```

### Step 6: Cleanup
```bash
# Stop and remove containers
docker stop container1 container2 container3
docker rm container1 container2 container3

# Remove the custom bridge network
docker network rm my-bridge-network
```

## Section 2: Advanced Network Management

### Connect an Existing Container to a Network
```bash
# Connect a running container to a network
docker network connect my-bridge-network existing-container
```

### Disconnect a Container from a Network
```bash
# Disconnect a container from a network
docker network disconnect my-bridge-network container-name
```

### Create a Network with Custom Subnet and Gateway
```bash
# Create a network with a specific subnet and gateway
docker network create --driver bridge \
  --subnet 172.28.0.0/16 \
  --gateway 172.28.0.1 \
  custom-subnet-network
```

### Assign Static IP to a Container
```bash
# Launch a container with a static IP address
docker run -d --name static-ip-container \
  --network=my-bridge-network \
  --ip=172.28.0.100 \
  nginx
```

## Section 3: Network Troubleshooting

### Inspect Network Details
```bash
# Inspect detailed information about a network
docker network inspect network-name
```

### Examine Container Network Settings
```bash
# Inspect network settings of a container
docker inspect container-name
```

### Test Connectivity with Ping
```bash
# Ping a container from another container
docker exec -it source-container ping target-container
```

### Use Netshoot for Advanced Troubleshooting
```bash
# Launch a container with Netshoot for network troubleshooting
docker run -it --network container:container-name nicolaka/netshoot
```

## Learning Notes

1. User-defined bridge networks provide better isolation and flexibility compared to the default bridge network.
2. Containers connected to the same network can communicate using their container names as hostnames.
3. Multiple containers can be launched and connected to a custom network simultaneously.
4. Network settings such as subnet and gateway can be customized during network creation.
5. Advanced network troubleshooting can be performed using tools like `docker network inspect` and `nicolaka/netshoot` container.

> 💡 **Best Practice**: Use meaningful and descriptive names for your custom bridge networks to ensure clarity and organization in your Docker environment.

> ⚠️ **Warning**: Be cautious when executing network-related commands, especially those involving network creation, deletion, or modification, as they can impact the connectivity and stability of your containers.

> 📝 **Note**: The commands provided in this reference guide are based on Docker version 20.10. Make sure to consult the official Docker documentation for any version-specific differences or updates.