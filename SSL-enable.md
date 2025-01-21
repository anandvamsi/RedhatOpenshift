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

$ oc get pods -n openshift-console

$ oc get route console -n openshift-console -o yaml
```
