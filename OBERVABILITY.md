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

![image](https://user-images.githubusercontent.com/59960562/125889416-38963027-58cb-418e-887b-675632ea2484.png)


### Liveness probe

can be configured to check if the container is healthy or not

![image](https://user-images.githubusercontent.com/59960562/125889430-9d65e1a1-3cb6-4caa-9b94-39e189ebe600.png)

livenessProbe

### Container Logging

kubectl logs <pod_name> <container_name>

### Monitoring

![image](https://user-images.githubusercontent.com/59960562/125889452-961822d3-8575-4574-90dc-21222c88aea0.png)

- we will focus metrics server only

### Metrics Server

- in memory
- cAdvisor
