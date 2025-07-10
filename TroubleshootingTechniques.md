
# Troubleshooting commands when the console is down.

- check for the memory and cpu limits.
- Mostly down due to cpu and memory pressure
- Bonus tip assign the pod with limits.


```bash
ssh core@<maser_node>
$ sudo -i
$ export KUBECONFIG=/etc/kubernetes/static-pod-resources/kube-apiserver-certs/secrets/node-kubeconfigs/localhost.kubeconfig
$ oc adm must-gather
```

## commands that needs to check inside a worker node
```bash
journalctl -u kubelet      # Kubelet logs
journalctl -u crio         # CRI-O logs (for containers)
journalctl -xe
```

## OC command to get pods from single host machine
```bash
oc get pods --all-namespaces --field-selector spec.nodeName=worker-node-1 -o wide
oc adm top pods
```

