## `cilium` network plugin manual
### Clean up `calico` (just in case)
Remove the calico binary and config file in each node:
```bash
sudo mv /etc/cni/net.d/10-calico.conflist /etc/cni/net.d/10-calico.conflist.cilium_bak
sudo rm $(which calicoctl)
```
And then restart the kubelet (run from control plane node):
```bash
sudo systemctl restart kubelet
```
List all calico CRDs:
```bash
sudo kubectl get crds -A | grep calico
```
<details><summary>outputs</summary>

```
bgpconfigurations.crd.projectcalico.org
bgppeers.crd.projectcalico.org
blockaffinities.crd.projectcalico.org
caliconodestatuses.crd.projectcalico.org
clusterinformations.crd.projectcalico.org
felixconfigurations.crd.projectcalico.org
globalnetworkpolicies.crd.projectcalico.org
globalnetworksets.crd.projectcalico.org
hostendpoints.crd.projectcalico.org
ipamblocks.crd.projectcalico.org
ipamconfigs.crd.projectcalico.org
ipamhandles.crd.projectcalico.org
ippools.crd.projectcalico.org
ipreservations.crd.projectcalico.org
kubecontrollersconfigurations.crd.projectcalico.org
networkpolicies.crd.projectcalico.org
networksets.crd.projectcalico.org
```
</details>
Delete all of them:
<pre>
sudo kubectl delete crd <b>CRD_NAME</b>
</pre>

Remove all `iptables` rules in each node:
```bash
sudo iptables -F
```

Delete calico deployment and daemonset:
```bash
sudo kubectl -n kube-system delete deployment calico-kube-controllers
sudo kubectl -n kube-system delete daemonset calico-node
```

Change the permission of `/opt/cni/bin` on each node to avoid errors in `mount-cgroup` container of `cilium` pods:
```bash
sudo chown -R root:root /opt/cni/bin
```

### Reboot nodes
Had to reboot all nodes after migrating, check the [migration note for version `v1.13`](https://docs.cilium.io/en/v1.13/installation/k8s-install-migration/) for more details (using [cilium version `1.12.1`](https://github.com/metalwhale/ocean/blob/58ac8e459bd28e5b86abe7d8158d3a526142f0d5/kubernetes/inventory/group_vars/k8s_cluster/k8s-net-cilium.yml#L2) but found no migration note for this version)

### Troubleshooting
If the following error occurs in the `mount-cgroup` container of cilium pods:
```
cp: cannot create regular file '/hostbin/cilium-mount': Permission denied
```
Run this command on each node to resolve ([ref](https://github.com/cilium/cilium/issues/23838#issuecomment-1434480322)):
```bash
sudo chown -R root:root /opt/cni/bin
```
