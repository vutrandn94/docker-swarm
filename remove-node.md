# Remove Swarm Node From Cluster

## Drain a node on the swarm (execute on 1 master node)
[References](https://docs.docker.com/engine/swarm/swarm-tutorial/drain-node/)
> [!TIP]
> Sometimes, such as planned maintenance times, you need to set a node to Drain availability. Drain availability prevents a node from receiving new tasks from the swarm manager. It also means the manager stops tasks running on the node and launches replica tasks on a node with Active availability.

**Syntax:**
```
# docker node update --availability drain <NODE_HOSTNAME>
```

**Lab:**
```
root@swarm-master-01:/home/ubuntu# docker node update --availability drain swarm-worker-03
swarm-worker-03

root@swarm-master-01:/home/ubuntu# docker node inspect --pretty swarm-worker-03
ID:			m9tajta4dgq8ozb3u2blfnbyq
Hostname:              	swarm-worker-03
Joined at:             	2025-08-18 03:45:05.338622765 +0000 utc
Status:
 State:			Ready
 Availability:         	Drain
 Address:		172.31.39.240
Platform:
 Operating System:	linux
 Architecture:		x86_64
Resources:
 CPUs:			2
 Memory:		3.82GiB
...

root@swarm-master-01:/home/ubuntu# docker node ls
ID                            HOSTNAME          STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
ptdood28n7rks22x4tfyire3p *   swarm-master-01   Ready     Active         Leader           28.3.3
yfna5w87trq4esw00tge8fwqw     swarm-master-02   Ready     Active         Reachable        28.3.3
nyysh4mvt5ltqj4xm5qwzs1e8     swarm-master-03   Ready     Active         Reachable        28.3.3
jk5t4bq0rowh62g70igein6t9     swarm-worker-01   Ready     Active                          28.3.3
6zt4zgnmq2g5km61y04mv11g5     swarm-worker-02   Ready     Active                          28.3.3
m9tajta4dgq8ozb3u2blfnbyq     swarm-worker-03   Ready     Drain                           28.3.3

```
> [!TIP]
> To return the drained node to an active state: "**docker node update --availability active <NODE_HOSTNAME>**"

## Remove node from cluster (execute on 1 master node)
**Syntax:**
```
# docker node rm <NODE_HOSTNAME> --force
```

**Lab:**
```
root@swarm-master-01:/home/ubuntu# docker node rm swarm-worker-03 --force
swarm-worker-03

root@swarm-master-01:/home/ubuntu# docker node ls
ID                            HOSTNAME          STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
ptdood28n7rks22x4tfyire3p *   swarm-master-01   Ready     Active         Leader           28.3.3
yfna5w87trq4esw00tge8fwqw     swarm-master-02   Ready     Active         Reachable        28.3.3
nyysh4mvt5ltqj4xm5qwzs1e8     swarm-master-03   Ready     Active         Reachable        28.3.3
jk5t4bq0rowh62g70igein6t9     swarm-worker-01   Ready     Active                          28.3.3
6zt4zgnmq2g5km61y04mv11g5     swarm-worker-02   Ready     Active                          28.3.3
```

## Clear status of worker node that has left the cluster (execute on worker node has left the cluster - OPTIONAL)

[References](https://docs.docker.com/reference/cli/docker/node/rm/)
> This document is useful if you want to reuse worker nodes.

**Syntax:**
```
# docker swarm leave
```
**Lab:**
```
root@swarm-worker-03:/home/ubuntu# docker swarm leave
Node left the swarm.
```
