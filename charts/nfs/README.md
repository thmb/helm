# NETWORK FILE SYSTEM

https://github.com/appscode/third-party-tools/tree/master/storage/nfs

https://github.com/kubernetes/minikube/issues/3417

For anyone else finding themselves in the same situation, who can't use the ClusterIP service, I was also able to get it to work using the NFS CSI Driver like @fosmjo mentioned above. Apparently v4.4.0 defaults to the necessary dnsPolicy as well, so no need for configuration beyond their default helm chart. Figured I'd drop a full example for copy pasta.

Installed the helm chart from their repo:

```console
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts
helm install csi-driver-nfs csi-driver-nfs/csi-driver-nfs --namespace kube-system
```

I'm running NFS inside my cluster using the gp2 StorageClass to create an EBS-backed volume for my deployment, here's my template:

# TODO: Improve this documentation to use either dynamic service name or static IP address

```console
helm install --set address=XXX.XXX.XXX.XXX nfs .
```

Test Client

kubectl exec test-client -- touch /nfs/data/demo.txt