## Create a secret 
```
oc create secret generic htpass-secret --from-file=/tmp/htpasswdfile -n openshift-config
```
