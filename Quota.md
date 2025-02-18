
##

## Labelling the namespace
```bash
oc new-project quotaexample
oc describe project quotaexample
Name:                   quotaexample
Created:                16 seconds ago
Labels:                 kubernetes.io/metadata.name=quotaexample
                        pod-security.kubernetes.io/audit=restricted
                        pod-security.kubernetes.io/audit-version=latest
                        pod-security.kubernetes.io/warn=restricted
                        pod-security.kubernetes.io/warn-version=latest
Annotations:            openshift.io/description=
                        openshift.io/display-name=
                        openshift.io/requester=kube:admin
                        openshift.io/sa.scc.mcs=s0:c28,c7
                        openshift.io/sa.scc.supplemental-groups=1000770000/10000
                        openshift.io/sa.scc.uid-range=1000770000/10000
Display Name:           <none>
Description:            <none>
Status:                 Active
Node Selector:          <none>
Quota:                  <none>
Resource limits:        <none>
oc label namespace quotaexample kubernetes.io/metadata=sample1
oc label namespace quotaexample zone=west
```

## to get the namespace name


## create clusterresource quota
```
oc create clusterresourcequota testquota1 --project-label-selector=zone=us-west --hard=pods=5 -o yaml > oc-clusterQ.yaml
oc create -f oc-clusterQ.yaml
```
