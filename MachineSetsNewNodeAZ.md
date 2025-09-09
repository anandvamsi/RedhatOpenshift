# Adding New node in New AZ

## Requirement
i have 4 worker nodes 2 in us-west-2a and 2 in us-west-2b , using machine set it possible to add a new machine in us-west-2c

### step 1:
```bash
oc get machineset -n openshift-machine-api
```

### step 2:
```bash
oc get machineset cluster-xyz-worker-us-west-2a -n openshift-machine-api -o yaml > machineset-2c.yaml
```
Replace all 2b with 2c and replace the 2b subnet with 2c subnet

### step 3:-
```bash
oc apply -f machineset-2c.yaml
oc scale machineset cluster-xyz-worker-us-west-2c -n openshift-machine-api --replicas=1
```

### Verification
```bash
oc get machines -n openshift-machine-api
```
