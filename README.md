# Docker Swarm all-in-one
| Use case | Tutorial file | 
| :--- | :--- |
| Deploy new cluster | [deploy-swarm-cluster.md](https://github.com/vutrandn94/docker-swarm/blob/main/deploy-swarm-cluster.md) |
| Remove node | [remove-node.md](https://github.com/vutrandn94/docker-swarm/blob/main/remove-node.md) |

## Command note
**Example apply / update 1 stack (declarative):**
```
# docker stack deploy -c stack-example.yaml example-app
```

**Check cluster node:**
```
# docker node ls
```

**Get stack:**
```
# docker stack ls
```

**Delelete 1 stack:**
```
# docker stack rm <STACK_NAME>

Example:
root@swarm-master-01:/home/ubuntu# docker stack rm example-app1
Removing service example-app1_app
Removing service example-app1_mysql8
Removing network example-app1_appnet1
```
**Get services created by 1 stack:**
```
#   docker stack services <STACK_NAME>

Example:
root@swarm-master-01:/home/ubuntu# docker stack services redis-app
ID             NAME              MODE         REPLICAS   IMAGE            PORTS
vzy2ymucu2gg   redis-app_redis   replicated   1/1        redis:7-alpine   
n4u7x0glef6r   redis-app_web     replicated   3/3        nginx:alpine     *:8081->80/tcp
```

**Delete 1 service:**
```
# docker service rm <SERVICE_NAME>

Example:
root@swarm-master-01:/home/ubuntu# docker service rm my-nginx
my-nginx
```

**Get service:**
```
# docker service ls
```

**Scale up / down 1 service:**
```
# docker service scale <SERVICE>=<REPLICAS>

Example:
root@swarm-master-01:/home/ubuntu# docker service scale redis-app_web=5
redis-app_web scaled to 5
overall progress: 5 out of 5 tasks 
1/5: running   [==================================================>] 
2/5: running   [==================================================>] 
3/5: running   [==================================================>] 
4/5: running   [==================================================>] 
5/5: running   [==================================================>] 
verify: Service redis-app_web converged 
```

**Example create service imperative:**
```
# docker service create --replicas 3 --name my-nginx --constraint 'node.role==worker' -p 8080:80 nginx:alpine
```
