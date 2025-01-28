# To remove the unnecessary fields from the message
## Use of prune.

```bash
apiVersion: observability.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  name: devocplogforwarder
  namespace: openshift-logging
spec:
  filters:
  - name: prune
    prune:
      in: [.kubernetes.annotations, .kubernetes.namespace_id,.kubernetes.container_id,.kubernetes.container_image,.kubernetes.container_image_id,.kubernetes.container_iostream,.kubernetes.container_name,.kubernetes.labels,.kubernetes.namespace_labels,.kubernetes.pod_id,.kubernetes.pod_owner,.kubernetes.pod_ip,.openshift]
    type: prune
  serviceAccount:
    name: logcollector
  outputs:
  - name: my-cw
    type: cloudwatch
    cloudwatch:
      groupName: ocp-np-cluster-{.log_type||"unknown"}
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
    filterRefs:
      - prune
    inputRefs:
      - application
      - infrastructure
    outputRefs:
      - my-cw
```
