### Via Docker Desktop

WARNING: This approach doesn't work right now due to linkerd not installing in a single node cluster.

Enable the single node Kubernetes cluster from the Settings page and apply + restart. If you already had it enabled please reset it to ensure a clean start.

Now switch `kubectl` context to the docker desktop cluster and label the node to accept all types of work-loads:

```
kubectl config use-context docker-desktop
kubectl label node docker-desktop node-role.kubernetes.io/worker=
kubectl label node docker-desktop node-role.kubernetes.io/database=

# Patch kube-prometheus-stack to avoid file system failure:
# https://github.com/prometheus-community/helm-charts/issues/467#issuecomment-1345563363
yq eval '.helmCharts[0].valuesInline.prometheus-node-exporter.hostRootFsMount.enabled = false' -i platform/kube-prometheus-stack/kustomization.yaml
git add platform/kube-prometheus-stack/kustomization.yaml
git commit -m "Disable hostRootFsMount for prometheus-node-exporter for Docker Desktop support"
git push
```

You can now proceed to the Installation section.
