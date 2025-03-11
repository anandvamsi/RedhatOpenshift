# Change the password of the kubeadmins

## Create a hash of the password ```bxYJn-jvZBy-viWJi-HGxZA```
```bash
htpasswd -bnBC 10 "" bxYJn-jvZBy-viWJi-HGxZA | cut -c 2- | base64
JDJ5JDEwJERwak5JQmR3cm45TndpdC9rUWs4TnVzclhxeGxGdkVJM0VLYWtwZVNLMEREOHFrYWQ3eWFhCgo=
```

## Update the password
```bash
oc patch -n kube-system secret/kubeadmin --patch '{"data": {"kubeadmin": "JDJ5JDEwJERwak5JQmR3cm45TndpdC9rUWs4TnVzclhxeGxGdkVJM0VLYWtwZVNLMEREOHFrYWQ3eWFhCgo="}}'
secret/kubeadmin patched
```
