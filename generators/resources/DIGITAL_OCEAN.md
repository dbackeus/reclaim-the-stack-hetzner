# Digital Ocean

## Create Kubernetes cluster

Manage -> Kubernetes -> Create a Kubernetes cluster

Choose a location and configure as you wish. For high availability deployments you'll need at least 3 nodes.

Create Cluster

Download the Kubeconfig file and merge it into your `.kube/config` file.

```
KUBECONFIG=~/.kube/config:path/to/cluster-kubeconfig.yaml kubectl config view --flatten > /tmp/kubeconfig
mv /tmp/kubeconfig ~/.kube/config
```

Now you can change the current kubectl context to the Digital Ocean Kubernetes cluster (look at the downloaded config file to see the name).

```
kubectl config use-context <name-of-kubeconfig-context>
```

Add node role labels:

```
kubectl label node <node-name> node-role.kubernetes.io/worker=
kubectl label node <node-name> node-role.kubernetes.io/database=
```


Notes:
- Appears to use cillium for networking
