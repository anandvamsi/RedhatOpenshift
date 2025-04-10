# How to scale the node in OCP Manually.
A MachineSet in OCP is like a Deployment, but for VMs (nodes) instead of pods.
It ensures a specific number of identical worker nodes (EC2 instances in AWS) are running and part of your OpenShift cluster.

### Verify the node with using the machine sets
```bash
oc get machinesets -n openshift-machine-api
NAME                          DESIRED   CURRENT   READY   AVAILABLE   AGE
poc1-b2r5x-worker-us-west-2a   1         1         1       1           33d
poc2-b2r5x-worker-us-west-2b   1         1         1       1           33d
poc3-b2r5x-worker-us-west-2c   1         1         1       1           33d
```

### Scale the node specifying the region 
```bash
oc scale machineset poc1-b2r5x-worker-us-west-2a --replicas=2 -n openshift-machine-api
machineset.machine.openshift.io/poc-b2r5x-worker-us-west-2a scaled
```

### Wait for the process 3-5 mins --- Look for provisioned status
```bash
oc get machines -n openshift-machine-api -w
NAME                                PHASE         TYPE          REGION      ZONE         AGE
poc1-b2r5x-master-0                  Running       m6i.2xlarge   us-west-2   us-west-2b   33d
poc1-b2r5x-worker-us-west-2a-4ml9r   Provisioned   m6i.xlarge    us-west-2   us-west-2a   40s
poc1-b2r5x-worker-us-west-2a-dm9vz   Running       m6i.xlarge    us-west-2   us-west-2a   33d
poc1-b2r5x-worker-us-west-2b-fflln   Running       r6a.xlarge    us-west-2   us-west-2b   33d
poc1-b2r5x-worker-us-west-2c-5cdx6   Running       m6i.xlarge    us-west-2   us-west-2c   33d
```
### Verify the nodes  - Make sure new node is addded
```bash
oc get nodes
```

### To verify pods of the newly added node
```bash
 oc get pods -A --field-selector spec.nodeName=ip-10-XX-81-136.corp.hds.com -o wide
```

## Scaling down

### cordon all node to which you want to scle down
```bash
oc adm cordon ip-10-XXX-81-136.corp.hds.com
node/ip-10-XXX-81-136.corp.hds.com cordoned
```
### Drain all the pods 
```bash
oc adm drain <node> --ignore-daemonsets
node/ip-10-80-81-136.corp.hds.com already cordoned
Warning: ignoring DaemonSet-managed Pods: amazon-cloudwatch/cwagent-5kblb, amazon-cloudwatch/fluent-bit-l4d4h, eai-test/node-exporter-cjh9q, openshift-cluster-csi-drivers/aws-ebs-csi-driver-node-56hsn, openshift-cluster-node-tuning-operator/tuned-n699g, openshift-dns/dns-default-dfvjw, openshift-dns/node-resolver-h2ws4, openshift-image-registry/node-ca-zmntc, openshift-ingress-canary/ingress-canary-chcvw, openshift-machine-config-operator/machine-config-daemon-47g8v, openshift-monitoring/node-exporter-5gtv6, openshift-multus/multus-4rrhc, openshift-multus/multus-additional-cni-plugins-jf72n, openshift-multus/network-metrics-daemon-gmxnp, openshift-network-diagnostics/network-check-target-6sgjq, openshift-network-operator/iptables-alerter-wcrb9, openshift-ovn-kubernetes/ovnkube-node-5dx4l
evicting pod openshift-operator-lifecycle-manager/collect-profiles-29071425-phzpd
evicting pod openshift-operator-lifecycle-manager/collect-profiles-29071395-z7t9t
evicting pod eai-test/xgslookup-fcf978848-8hfkz
evicting pod openshift-operator-lifecycle-manager/collect-profiles-29071410-p6ftn
pod/xgslookup-1-build evicted
pod/emailservice-1-build evicted
pod/collect-profiles-29071410-p6ftn evicted
pod/collect-profiles-29071425-phzpd evicted
pod/collect-profiles-29071395-z7t9t evicted
pod/xgslookup-fcf978848-8hfkz evicted
pod/emailservice-578c8dbd95-bd84c evicted
node/ip-10-XXXXX81-136.corp.hds.com drained
```

### Checking the nodes status
```bash
oc get nodes
NAME                           STATUS                     ROLES                  AGE   VERSION
ip-10-XX-81-132.corp.hds.com   Ready                      worker                 34d   v1.30.5
ip-10-XX-81-136.corp.hds.com   Ready,SchedulingDisabled   worker                 75m   v1.30.5
ip-10-XX-81-169.corp.hds.com   Ready                      control-plane,master   34d   v1.30.10
ip-10-XX-81-178.corp.hds.com   Ready                      worker                 34d   v1.30.5
ip-10-XX-81-217.corp.hds.com   Ready                      worker                 34d   v1.30.5
```

### Now scale to old number, in our case it will 1----validate the region
```bash
oc scale machineset poc-b2r5x-worker-us-west-2a --replicas=1 -n openshift-machine-api
```

