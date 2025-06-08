# Kubernetes Cheat Sheet

## Commands

### Pods

List the pods in the default namespace:
```
kubectl get pods
```

List the pods in a specific namespace:
```
kubectl get pods -n <namespace>
```

List pods with more information: 
```
kubectl get pods -o wide [-n <namespace>]
```

See a detailed description of a pod:
```
kubectl describe pod <pod-name> [-n <namespace>]
```

See the logs for a pod:
```
kubectl logs <pod-name> [-n <namespace>]
```

Execute a command on a pod (often used for getting a terminal):
```
kubectl exec -it <pod-name> [-n namespace] -- /bin/sh
```
Note: replace */bin/sh* with a different flavour shell if preferred