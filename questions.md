![image](https://user-images.githubusercontent.com/59960562/125798129-eab83825-1180-4a15-9b0d-bd1073341336.png)

[21:02, 15/07/2021] Farhan: 1- what i did is kubectl get pods
[21:02, 15/07/2021] Farhan: 2- then kubectl describe pod <podname>
[21:02, 15/07/2021] Farhan: to see it labels
[21:02, 15/07/2021] Farhan: 3- and then open docs
  
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
           name: mysql
    ports:
    - protocol: TCP
      port: 3306
  - to:
    - podSelector:
        matchLabels:
            name: payroll
    ports:
    - protocol: TCP
      port: 8080
