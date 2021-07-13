# POD DESIGN
## Labels and selectors

## Labels

- labels are property attach to services

## Selectors

- to select object

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a25dca73-b919-4701-b621-303641ea5f6f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a25dca73-b919-4701-b621-303641ea5f6f/Untitled.png)

kubectl get pods —selector app=App1

- below is label for pod
- top is label for the service
- we use selector to match between labels and pod

### Annotations

annotations:

   buildversion: 

### Rollout and Versioning

- revision 1 ,revision 2 ← rollout revision
- k rollout status deployment/myapp-deployment
- k rollout history deployment/myapp-deployment

### DEployment strategy

- recreate
- rolling update(default)
- kubectl apply -f deployment-definition.yaml
- kubectl set image deployment/myapp-deployment nginx=nginx<version>

Upgrades under the hood

- create new replicate set under the hood and create container there
- kubectl get replicasets

Rollback

- kubectl rollout undo deployment/<deplloymentname>

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/85b73660-58fb-4fdf-8b16-2969179164d5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/85b73660-58fb-4fdf-8b16-2969179164d5/Untitled.png)

Here are some handy examples related to updating a Kubernetes Deployment:

- **Creating a deployment, checking the rollout status and history:**

In the example below, we will first **create** a simple deployment and inspect the **rollout status** and the **rollout history**:

`1. master $ kubectl create deployment nginx --image=nginx:1.16
2. deployment.apps/nginx created
3.  
4. master $ kubectl rollout status deployment nginx
5. Waiting for deployment "nginx" rollout to finish: 0 of 1 updated replicas are available...
6. deployment "nginx" successfully rolled out
7.  
8. master $
9.  
10.  
11. master $ kubectl rollout history deployment nginx
12. deployment.extensions/nginx
13. REVISION CHANGE-CAUSE
14. 1     <none>
15.  
16. master $`

- **Using the --revision flag:**

Here the revision 1 is the first version where the deployment was created.

You can check the status of each revision individually by using the **--revision flag**:

`1. master $ kubectl rollout history deployment nginx --revision=1
2. deployment.extensions/nginx with revision #1
3.  
4. Pod Template:
5.  Labels:    app=nginx    pod-template-hash=6454457cdb
6.  Containers:  nginx:  Image:   nginx:1.16
7.   Port:    <none>
8.   Host Port: <none>
9.   Environment:    <none>
10.   Mounts:   <none>
11.  Volumes:   <none>
12. master $`

- **Using the --record flag:**

You would have noticed that the "**change-cause**" field is empty in the rollout history output. We can use the **--record flag** to save the command used to create/update a deployment against the revision number.

`1. master $ kubectl set image deployment nginx nginx=nginx:1.17 --record
2. deployment.extensions/nginx image updated
3. master $master $
4.  
5. master $ kubectl rollout history deployment nginx
6. deployment.extensions/nginx
7.  
8. REVISION CHANGE-CAUSE
9. 1     <none>
10. 2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
11. master $`

You can now see that the **change-cause** is recorded for the revision 2 of this deployment.

Let's make some more changes. In the example below, we are editing the deployment and changing the image from **nginx:1.17** to **nginx:latest** while making use of the --record flag.

`1. master $ kubectl edit deployments. nginx --record
2. deployment.extensions/nginx edited
3.  
4. master $ kubectl rollout history deployment nginx
5. REVISION CHANGE-CAUSE
6. 1     <none>
7. 2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
8. 3     kubectl edit deployments. nginx --record=true
9.  
10.  
11.  
12. master $ kubectl rollout history deployment nginx --revision=3
13. deployment.extensions/nginx with revision #3
14.  
15. Pod Template: Labels:    app=nginx
16.     pod-template-hash=df6487dc Annotations: kubernetes.io/change-cause: kubectl edit deployments. nginx --record=true
17.  
18.  Containers:
19.   nginx:
20.   Image:   nginx:latest
21.   Port:    <none>
22.   Host Port: <none>
23.   Environment:    <none>
24.   Mounts:   <none>
25.  Volumes:   <none>
26.  
27. master $`

- **Undo a change:**

Lets now rollback to the previous revision:

`1. master $ kubectl rollout undo deployment nginx
2. deployment.extensions/nginx rolled back
3.  
4. master $ kubectl rollout history deployment nginx
5. deployment.extensions/nginxREVISION CHANGE-CAUSE
6. 1     <none>
7. 3     kubectl edit deployments. nginx --record=true
8. 4     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
9.  
10.  
11.  
12.  
13. master $ kubectl rollout history deployment nginx --revision=4
14. deployment.extensions/nginx with revision #4Pod Template:
15.  Labels:    app=nginx    pod-template-hash=b99b98f9
16.  Annotations: kubernetes.io/change-cause: kubectl set image deployment nginx nginx=nginx:1.17 --record=true
17.  Containers:
18.   nginx:
19.   Image:   nginx:1.17
20.   Port:    <none>
21.   Host Port: <none>
22.   Environment:    <none>
23.   Mounts:   <none>
24.  Volumes:   <none>
25.  
26.  
27. master $ kubectl describe deployments. nginx | grep -i image:
28.   Image:    nginx:1.17
29. master $`

With this, we have rolled back to the previous version of the deployment with the **image = nginx:1.17.**

kenapa kalau inline command tak type record dia record, tapi bila kita nak create or apply kene specify record

kbernetes want the pod to live forever

### RestartPolicy : never

kubernetes jobs

batch/v1 

kind: Job

completions : ?

parallelism : 3

### CronJobs

batch/v1beta1
kind: CronJob
spec:
  schedule:
  jobTemplate:
    spec: