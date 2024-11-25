# How to convert CLB to NLB
By default when you install OCP cluster, CLB in installed you can replace the CLB with NLB using the ingress controller.

```bash
apiVersion: operator.openshift.io/v1
kind: IngressController
metadata:
  creationTimestamp: null
  name: default
  namespace: openshift-ingress-operator
spec:
  endpointPublishingStrategy:
    loadBalancer:
      scope: Internal
      providerParameters:
        type: AWS
        aws:
          type: NLB
    type: LoadBalancerService
```

```bash
oc apply -f clb-nlb.yaml
ingresscontroller.operator.openshift.io/default configured
```
Once you have applied this, replace the new NLB URL in the DNS (Route53)
