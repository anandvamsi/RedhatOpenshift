helm installation
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# TO create helm chart 
helm create nginx-demo
helm version


#cd nginx-demo
nginx-demo/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    └── _helpers.tpl




#cat > values.yaml
apiVersion: v2
name: nginx-demo
description: A Helm chart for deploying nginx
type: application
version: 0.1.0
appVersion: "1.25"


#cat > values.yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.25"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: false
  annotations: {}
  hosts:
    - host: nginx.local
      paths:
        - path: /
          pathType: Prefix
  tls: []

resources: {}

nodeSelector: {}

tolerations: []

affinity: {}



#cat > templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "nginx-demo.fullname" . }}
  labels:
    app: {{ include "nginx-demo.name" . }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "nginx-demo.name" . }}
  template:
    metadata:
      labels:
        app: {{ include "nginx-demo.name" . }}
    spec:
      containers:
        - name: nginx
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: {{ .Values.service.port }}


# cat > templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "nginx-demo.fullname" . }}
spec:
  type: {{ .Values.service.type }}
  selector:
    app: {{ include "nginx-demo.name" . }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.port }}


# Deployment 
helm install my-nginx ./nginx-demo

# To expose the app
oc expose svc/my-nginx-nginx-demo
route.route.openshift.io/my-nginx-nginx-demo exposed

