# Adding users via httpdpasswd

## Create user with httpasswd command
```bash
htpasswd -c -B -b htpasswdall1 anand anandXXXX
```
Note:- Dont use '$' in the password

## Create a secret with new secret name and passwd
```bash
oc create secret generic htpasswdalldevupdated --from-file=htpasswd=htpasswdall1 -n openshift-config
```

## Create the manifest file with above secret
```bash
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: my_htpasswd_provider
    mappingMethod: claim
    type: HTPasswd
    htpasswd:
      fileData:
        name: htpasswdalldevupdated
```
## Apply the secret to the cluster
```bash
oc apply -f httpaswd.yaml
```

### Wait for 60-90 seconds

### Login to the cluster
```bash
oc login -u <newuser>
```


## if we have requirement to add new user

```bash
htpasswd -c -B -b htpasswdsrikanth srikanth1 srikanth13XXX45
```

Edit the existing ocp/kubernetes secret and append the new user entry. 
