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

#### Resources
Apply resource limits to your deployed pods to prevent a single pod consuming all the available cpu/memory on a node which can cause the node to fail and cause outages.
Resources are applied to the *containers* section of a k8s configuration file and can look something like this:
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```
*Requests* states: "don't schedule this pod unless the resource min is met"
*Limits* states: "stop running this pod if these limits are exceeded"
See an example of a configuration file with resource limits [here](deployments/quote-example-with-limits.yaml)

### Apply

*kubectl apply* is used for _applying_ a configuration to the k8s cluster defined in a yaml or json file.
Examples of theses files can be found throughout the repo, for example the *deployments* and *services* folders.

To apply a configuration in a specific file:
```
kubectl apply -f <configuration-file>.json/yaml
```

## Terminology

*Cluster* - An instance of Kubernetes

*Control Plane* - The "air traffic controller" of the cluster which ensures that node and pods are created, modified and delete correctly

*Kube Api Server* - A Control Plane component which exposed the k8s api which in terms talks to the pods api endpoints. `kubectl` and `kubeadm` are the CLI abstraction to the `Kube Api Server`.