## OBSERVABILITY
### Readiness and liveness probes

PodSchedules
Initialized
ConatinersReady
Ready

kubectl describe pod - see at conditions section

containersready = 

Readiness Probes - HTTP Test /api/ready

configure test
readinessProbe:

httpGet:

path: /api/ready

port: 8080

TCP? port 3306 

command - cat -/app/is_ready (script)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d426fb94-dfda-4c18-ad3c-1916f0a85190/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d426fb94-dfda-4c18-ad3c-1916f0a85190/Untitled.png)

### Liveness probe

can be configured to check if the container is healthy or not

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2da5199d-89ba-4e13-bfdb-cc69d9824252/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2da5199d-89ba-4e13-bfdb-cc69d9824252/Untitled.png)

livenessProbe

### Container Logging

kubectl logs <pod_name> <container_name>

### Monitoring

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e1ef86e-6412-4e10-92f7-928b1e6776e6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e1ef86e-6412-4e10-92f7-928b1e6776e6/Untitled.png)

- we will focus metrics server only

### Metrics Server

- in memory
- cAdvisor