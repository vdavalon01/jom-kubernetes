# Tips, yaml

Mistakes easily made, thus

- dont use tab
- always put like double space if it is not array

---

# in vim

```
:setlocal ts=2 sts=2 sw=2 expandtab
```

source: https://stackoverflow.com/questions/26962999/wrong-indentation-when-editing-yaml-in-vim








---

# k8s imperative outputs yaml

```
kubectl run nginx --image=nginx --dry-run=client -o yaml > deployment.yaml
vim deployment.yaml
```
