# Nginx Docker Setup with Persistent Logs and Custom Network

## Overview
Configure **Nginx server** using **Docker Compose**, ensuring that logs persist between container restarts and that the container runs on a custom **bridge network (172.20.8.0/24)**.

## Prerequisites
Before running this setup, ensure you have:
- **Ubuntu 20.04 LTS**
- **Docker 19+** installed

## Installation
### Step 1: Install Docker and Docker Compose
Run the following commands to install Docker and Docker Compose:

```bash
sudo apt update
sudo apt install -y docker.io
```

Then, install Docker Compose:

```bash
sudo apt install -y docker-compose
```

Verify installation:

```bash
docker --version
docker-compose --version
```

---

### Step 2: Create a Custom Docker Network
We will create a custom **bridge network** with a predefined subnet **172.20.8.0/24**:

```bash
sudo docker network create --driver=bridge --subnet=172.20.8.0/24 nginx_network
```

Verify the network:

```bash
docker network inspect nginx_network
```

---

### Step 3: Create `docker-compose.yml`
Create a `docker-compose.yml` file:

```bash
nano docker-compose.yml
```

Add the following content:

```yaml
networks:
  nginx_network:
    external: true

services:
  nginx:
    image: nginx:latest
    container_name: nginx_server
    restart: always
    networks:
      nginx_network:
        ipv4_address: 172.20.8.2
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./logs:/var/log/nginx  # Persistent logs
      - ./nginx.conf:/etc/nginx/nginx.conf:ro  # Custom Nginx configuration (Optional)
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "5"
```

---


### Step 4: Start the Nginx Server
Run the following command to start the container:

```bash
docker-compose up -d
```

Verify that the container is running:

```bash
docker ps
```

---

### Step 5: Test and Verify
- **Check Logs:**
  ```bash
  ls -lah logs/
  ```
- **Restart Container & Check Logs:**
  ```bash
  docker-compose restart
  cat logs/access.log
  cat logs/error.log
  ```

---

# Kubernetes Troubleshooting

## Overview
How to identify the reason for a pod restart and analyzing potential causes of unexpected restarts for a Java-based application deployed in Kubernetes.

---

## 1. Identifying the Reason for a Pod Restart  
In Kubernetes, a pod may restart due to various reasons, including:  

- **Out of Memory (OOM)**: The pod exceeds its allocated memory limit and gets terminated by Kubernetes. 
- **Liveness Probe Failures**: If the liveness probe fails multiple times, Kubernetes restarts the pod.  
- **Application Crashes**: If the application inside the container crashes due to an unhandled exception or fatal error, the pod restarts.  
- **Node Resource Constraints**: If the node runs out of CPU, memory, or disk, Kubernetes may evict or restart the pod.  
 

To determine why a pod is restarting in the **"internal" project under the "production" namespace**, follow these steps:

### **Step 1: Use `kubectl describe pod` to Check Restart Events**
Run the following command to inspect the pod details, including restart reasons:

```sh
kubectl describe pod java-app-7d9d44ccbf-lmvbc -n production
```

### **Step 2: Use `kubectl logs` to Check Restart Logs**
If the pod has restarted, its logs may have been cleared. To view logs from the previous container instance:

```sh
kubectl logs java-app-7d9d44ccbf-lmvbc -n production --previous
```