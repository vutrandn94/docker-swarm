# DEPLOY DOCKER SWARM CLUSTER

## Lab info (3 master, 3 worker)
[References](https://docs.docker.com/engine/swarm/swarm-tutorial/)
| Hostname | Private IP Address | Public IP Mapping | OS | Role | Port Open |
| :--- | :--- | :--- | :--- | :--- | :--- |
| swarm-master-01 | 172.31.40.129 | 13.229.86.92 | Ubuntu 22.04.5 LTS | master | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 
| swarm-master-02 | 172.31.43.47 | 54.251.65.4 | Ubuntu 22.04.5 LTS | master | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 
| swarm-master-03 | 172.31.32.62 | 13.229.234.103 | Ubuntu 22.04.5 LTS | master | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 
| swarm-worker-01 | 172.31.41.220 | 54.254.254.50 | Ubuntu 22.04.5 LTS | worker | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 
| swarm-worker-02 | 172.31.38.200 | 13.229.217.243 | Ubuntu 22.04.5 LTS | worker | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 
| swarm-worker-03 | 172.31.39.240 | 18.142.243.89 | Ubuntu 22.04.5 LTS | worker | 2377(TCP), 7946 (TCP/UDP), 4789 (UDP) | 

## Set hostname for all nodes (master & worker)
**Syntax:**
```
# hostnamectl set-hostname <HOSTNAME>

# hostname
```
**Example:**
```
root@swarm-master-01:/home/ubuntu# hostnamectl set-hostname swarm-master-01
root@swarm-master-01:/home/ubuntu# hostname
swarm-master-01
```
## Install docker & docker compose for all nodes (master & worker)
[References](https://docs.docker.com/compose/install/standalone/)
```
# apt-get update
# apt-get install ca-certificates curl
# install -m 0755 -d /etc/apt/keyrings
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
# chmod a+r /etc/apt/keyrings/docker.asc

# echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# apt-get update
# apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# curl -SL https://github.com/docker/compose/releases/download/v2.39.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
# chmod +x /usr/local/bin/docker-compose
# systemctl restart docker && systemctl enable docker
```

## Init Swarm Cluster (config on node swarm-master-01)
**Syntax:**
```
# docker swarm init --advertise-addr <Private IP Address>
```

**Lab:**
```
root@swarm-master-01:/home/ubuntu# docker swarm init --advertise-addr 172.31.40.129
Swarm initialized: current node (ptdood28n7rks22x4tfyire3p) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token <WORKER_TOKEN_JOIN> 172.31.40.129:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions
```
**Copy command return output to join worker node.**

## Get join command & token for master node (execute on node swarm-master-01)
**Syntax:**
```
# docker swarm join-token manager
```

**Lab:**
```
root@swarm-master-01:/home/ubuntu# docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token <MASTER_TOKEN_JOIN> 172.31.40.129:2377
```
**Copy command return output to join master node.**

> [!TIP]
> To retrive command and token join for master node: "**docker swarm join-token manager**", To retrive command and token join for worker node: "**docker swarm join-token worker**"

## Join remaining master node to cluster (execute on node swarm-master-02 and swarm-master-03)
**Copy and paste command return output to join master node**
```
# docker swarm join --token <MASTER_TOKEN_JOIN> 172.31.40.129:2377
```

## Join remaining worker node to cluster (execute on all worker node)
**Copy and paste command return output to join worker node**
```
# docker swarm join --token <WORKER_TOKEN_JOIN> 172.31.40.129:2377
```

## Check cluster status (execute on 1 master node)
```
root@swarm-master-01:/home/ubuntu# docker node list
ID                            HOSTNAME          STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
ptdood28n7rks22x4tfyire3p *   swarm-master-01   Ready     Active         Leader           28.3.3
yfna5w87trq4esw00tge8fwqw     swarm-master-02   Ready     Active         Reachable        28.3.3
nyysh4mvt5ltqj4xm5qwzs1e8     swarm-master-03   Ready     Active         Reachable        28.3.3
jk5t4bq0rowh62g70igein6t9     swarm-worker-01   Ready     Active                          28.3.3
ysp7b50xa3i8pe238s4zem1d3     swarm-worker-02   Ready     Active                          28.3.3
m9tajta4dgq8ozb3u2blfnbyq     swarm-worker-03   Ready     Active                          28.3.3
```
