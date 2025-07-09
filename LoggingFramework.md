# How to ship logs to cloudwatch logs from openshift

- step 1: create create IAM keys for openshift log forwarder.
- setp 2: Install the operator  Red Hat OpenShift Logging
- step 3: create service accounts for same.
```bash
oc adm policy add-cluster-role-to-user collect-application-logs system:serviceaccount:openshift-logging:logcollector
oc adm policy add-cluster-role-to-user collect-infrastructure-logs system:serviceaccount:openshift-logging:logcollector
oc adm policy add-cluster-role-to-user collect-audit-logs system:serviceaccount:openshift-logging:logcollector
```
oc command to create sa list the service accounts
```
oc create sa logcollector -n openshift-logging
serviceaccount/logcollector created
anand@PF39MdfdfKQN:~/K8s/ocp-prod/CW$ oc get sa -n openshift-logging
NAME                       SECRETS   AGE
builder                    1         23h
cluster-logging-operator   1         23h
collector                  1         5m14s
default                    1         23h
deployer                   1         23h
logcollector               1         6s
```

- step 4: create the secrets
```bash
oc create secret generic cw-secret   --from-literal=aws_access_key_id=XXXX   --from-literal=aws_secret_access_key=XXXXXXXXXXXXXXOm   --namespace=openshift-logging
```
- step 5:- create the cluster log forwader.
  
```bash
apiVersion: observability.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  name: devocplogforwarder
  namespace: openshift-logging
spec:
  serviceAccount:
    name: cluster-logging-operator
  outputs:
  - name: my-cw
    type: cloudwatch
    cloudwatch:
      groupName: my-cluster-{.log_type||"unknown"}
      region: us-west-2
      authentication:
        type: awsAccessKey
        awsAccessKey:
          keyId:
            secretName: cw-secret
            key: aws_access_key_id
          keySecret:
            secretName: cw-secret
            key: aws_secret_access_key
  pipelines:
  - name: my-cw-logs
    inputRefs:
      - application
      - infrastructure
    outputRefs:
      - my-cw
```
- step 6 : Execute the log fowarder
```bash
oc apply -f clf-updater.yaml
```
