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

### Security
In the `spec -> containers` section of the configuration file, the following section can be added to strengthen the security of the deployment:
``` yaml
securityContext: 
  allowPrivilegeEscalation: false
  runAsNonRoot: true
  capabilities:
    drop:
      - ALL
  readOnlyRootFileSystem: true
```
See an [example](deployments/quote-example-with-security.yaml) of the security settings in a configuration file.

Another step to a more secure setup is to use an tool like [snyk cli](https://snyk.io) to scan for vulnerabilities in you manifest files.
Snyk is free for individual developers and can be install using brew:
```
brew install snyk-cli
```
Authenticate with snyk using:
```
snyk auth
```
>NOTE: If this is your first time running snyk, you need to setup your profile and then run `snyk auth` again to allow the cli integration.
To check a manifest file for vulnerabilities using snyk-cli run:
```
snyk iac test <filename>
```

## Data Storage / Stateful Applications
There are two ways of persisting data in your k8s running application
- Setting up a database independent of the k8s cluster 
- Using [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumesComponent/)

## Terminology

| Term      | Definition                                      |
|-----------|-------------------------------------------------|
| **Cluster** | An instance of Kubernetes |
| **Control Plane** | The "air traffic controller" of the cluster which ensures that node and pods are created, modified and delete correctly |
| **Kube Api Server** | A Control Plane component which exposed the k8s api which in terms talks to the pods api endpoints. `kubectl` and `kubeadm` are the CLI abstraction to the `Kube Api Server` |
| **Etcd** | A Control Plane key/value store with information about the cluster. *etcd* is also running as a pod |
| **Kube Scheduler** | Identifies newly created pods and choses a node for the pod to run on. Also runs as a pod |
| **Controller Manager** | Runs as a loop and inspects the condition of the cluster, for example checking that all worker nodes are up and if not replaces it |
| **Cloud Controller Manager** | Allows you to connect a k8s cluster to a cloud provider api |
| **Worker Node** | Where pods are scheduled and run. Worker nodes contain three components: `kubelet`, `cri` and `kube-proxy` |
| **Kubelet** | Agent on the worker node which monitors the pods health and communicate the status to the `Kube Api Server` in the control pane |
| **Container Runtime Interface (CRI)** | The `Kubelet` is scheduling/starting the containers using the Container Runtime Interface |
| **Kube-proxy** | Ensures that pods and services can communicate on the worker node |
| **DaemonSet** | Will put one copy of a container on each node in a cluster |
