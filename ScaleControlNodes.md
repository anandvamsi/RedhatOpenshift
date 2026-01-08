# Scale control node
How to scale worker node by changing the control node instance type

- Login to cli using token
- Execute below command and chance the instance type in the file and save it
  ```bash
  oc edit controlplanemachineset cluster -n openshift-machine-api
  ```
- Note:- one by one control node will get reflected you can validate the you can validate using below command
```bash
watch -n3 "oc get machine -n openshift-machine-api -o wide
```
