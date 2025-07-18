# How to enable SSL certificate to console URL

considering the web console URL is ```apps.eainonprod2.abc.live``` create a certificate for ```*.apps.eainonprod2.abc.live```

```bash
 oc create secret tls custom-tls-secret \
    --cert=</path/to/cert.crt> \
    --key=</path/to/key.key> \
    -n openshift-config

Next, patch the console operator conf

$ oc patch consoles.operator.openshift.io cluster \
    --type=merge -p \
    '{"spec":{"route":{"secret":{"name":"custom-tls-secret"}}}}'

$ oc get consoles.operator.openshift.io cluster -o yaml

$ 
oc get pods -n openshift-console
NAME                        READY   STATUS    RESTARTS   AGE
console-5f469865f5-8sd6h    1/1     Running   0          87m
console-5f469865f5-dtmcb    1/1     Running   0          87m
downloads-65f56d785-fzfjw   1/1     Running   0          90m
downloads-65f56d785-s8qps   1/1     Running   0          90m

$ oc get route console -n openshift-console -o yaml
```


## How to configure for ingress certificate
```bash
Please create a complete certificate chain including
- Domain cert
- Intermediate cert
- Root CA cert
$ cat domain.crt intermediate.crt root.crt > full-chain.crt


oc create configmap configmap-ingress-ca --from-file=ca-bundle.crt=full-chain.crt  -n openshift-config
configmap/configmap-ingress-ca created

oc patch proxy/cluster --type=merge --patch='{"spec":{"trustedCA":{"name":"configmap-ingress-ca"}}}'
proxy.config.openshift.io/cluster patched

oc create secret tls ingress-secret --cert=full-chain.crt --key=domain.key -n openshift-ingress
secret/ingress-secret created

oc patch ingresscontroller.operator default \
     --type=merge -p \
     '{"spec":{"defaultCertificate": {"name": "ingress-secret"}}}' \
     -n openshift-ingress-operator
ingresscontroller.operator.openshift.io/default patched
```
