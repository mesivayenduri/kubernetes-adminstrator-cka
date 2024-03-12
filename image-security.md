docker login private-registry.io


docker run private-registry.io/apps/internal-app



How does kubernetes can pull the images from a private registry with out knowing the credentials...

You have to create a secrets "docker-registry"

kuebctl create secret docker-registry regcred \
    --docker-server=private-registry.io \
    --docker-username=username \
    --docker-password=password \
    --docker-email=email


Eg: kubectl create secret docker-registry private-reg-cred --docker-server=myprivateregistry.com:5000 --docker-username=dock_
user --docker-password=dock_password --docker-email=dock_user@myprivateregistry.com



# Now you can update the imagePullSecrets in the deployment


apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "3"
  creationTimestamp: "2024-03-12T09:30:36Z"
  generation: 3
  labels:
    app: web
  name: web
  namespace: default
  resourceVersion: "3575"
  uid: 9e216d2a-a54f-4bb4-b769-192fe125e60c
spec:
  progressDeadlineSeconds: 600
  replicas: 2
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: web
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: web
    spec:
      containers:
      - image: myprivateregistry.com:5000/nginx:alpine
        imagePullPolicy: IfNotPresent
        name: nginx
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      imagePullSecrets:
      - name: private-reg-cred
      restartPolicy: Always
      schedulerName: default-scheduler