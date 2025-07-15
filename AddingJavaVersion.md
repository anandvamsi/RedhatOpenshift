# How to add new java version to build section

## Import an image and set an ImageStreamTag:
```bash
$ oc project openshift
$ oc import-image ubi8/openjdk-21:1.20-3.1724181172 --from=registry.access.redhat.com/ubi8/openjdk-21:1.20-3.1724181172 --confirm
$ oc tag openjdk-21:1.20-3.1724181172 openjdk-21:latest
```

## Openshift UI(Administrator-->Build-->ImageStream---(Search for java))
### Edit the Java ImageStream on the openshift namespace, as below, by adding a new tag with the custom name. Example:
```bash
    - name: openjdk-21 <-- Display name
      annotations:
        description: Build and run Java applications using Maven.
        iconClass: icon-rh-openjdk
        openshift.io/display-name: Java (OpenJDK21)
        sampleContextDir: undertow-servlet
        sampleRepo: 'https://github.com/jboss-openshift/openshift-quickstarts' <---- Index Repo
        supports: java
        tags: 'builder,java,openjdk'
        version: latest
      from:
        kind: ImageStreamTag
        name: 'openjdk-21:latest'
```
