### Arguments in docker and kubernetes
entry point and array format

- done

### ConfigMaps

- to store ENV data
    - create configmap
    - inject to the pop

    imperatibe

    kubectl create configmap \ app-config —from-literal=APP_COLOR=blue

    kubectl create configmap \ app-config —from-file

    configmap.yaml

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63ffd6a0-47e7-4436-ad8e-df5f2f2b30a2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63ffd6a0-47e7-4436-ad8e-df5f2f2b30a2/Untitled.png)

    kubectl create secret generic \ app-secret 

    echo -n 'mysql' | base 64

    - secretRef
        - name:

    ### SecurityContext

    - docker security??
        - ps aux

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb98f24a-f062-4924-ae7c-0a5437b78290/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb98f24a-f062-4924-ae7c-0a5437b78290/Untitled.png)

    ### ServiceAccounts

    - user accounts human
    - service account = machine
    - use service account!! to get pod berapa berapa and to monitor

    ### Resource Requirements

    - insufficient cpu the pod will be on pending state

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ada2313-efa3-4a13-a2cc-e42f0d8346ec/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ada2313-efa3-4a13-a2cc-e42f0d8346ec/Untitled.png)

    In the previous lecture, I said - "When a pod is created the containers are assigned a default CPU request of .5 and memory of 256Mi". For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.

    `1. apiVersion: v1
    2. kind: LimitRange
    3. metadata:
    4.   name: mem-limit-range
    5. spec:
    6.   limits:
    7.   - default:
    8.       memory: 512Mi
    9.     defaultRequest:
    10.       memory: 256Mi
    11.     type: Container`

    [https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)

    `1. apiVersion: v1
    2. kind: LimitRange
    3. metadata:
    4.   name: cpu-limit-range
    5. spec:
    6.   limits:
    7.   - default:
    8.       cpu: 1
    9.     defaultRequest:
    10.       cpu: 0.5
    11.     type: Container`

    [https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/)

    ### Taints and Tolerations

    # t**aint are set on node**

    # tolerant are set on pods

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3cb93ed-0d65-4cd1-a76d-29e17878b460/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3cb93ed-0d65-4cd1-a76d-29e17878b460/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f51696e-5e4a-407c-8fb8-ac9370fcff92/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f51696e-5e4a-407c-8fb8-ac9370fcff92/Untitled.png)

    taint - NoExecute

    master node ? 

    - scheduler does not schedule any pod on the master

    ### NodeSelectors

     size: Large

    kubectl label nodes node-1 size=Large

    ### Node Affinity

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f33c709-6ed1-48a9-b3b7-d5be7222c722/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f33c709-6ed1-48a9-b3b7-d5be7222c722/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ad9792f-1f50-42e5-b274-0c9ae9241b0f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ad9792f-1f50-42e5-b274-0c9ae9241b0f/Untitled.png)