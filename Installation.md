# Openshift Installation


## Prerequites
- Sign up for a free account with Redhat and Dowload the binary and create a passkey

- Download the openshift Binary
```bash
wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/stable/openshift-install-linux.tar.gz
tar -xvzf openshift-install-linux.tar.gz
```
- Create a domain and configure the same in the Route53 by creating public zone
## Create a ssh keypair
```bash
ssh-keygen -t ed25519 -N ''
ssh-add /home/ec2-user/.ssh/id_ed25519
```

## Preparing the Installer config
```bash
./openshift-install create install-config --dir /opt/ocp-installer/
```
- This will create install install-config.yaml in the directory /opt/ocp-installer/

## Creating a cluster. 
```bash
./openshift-install create cluster --dir=/opt/ocp-installer/ --log-level=debug
```

## Destorying the cluster
```bash
./openshift-install destroy cluster --dir=/opt/ocp5
```
