kubectl create serviceaccount dashboard-sa

Create a role and rolebinding with appropriate persmissions to view, list and watch the pods and bind it serviceaccount created in the previous step

kubectl get role pod-reader -o yaml

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: "2024-03-12T09:08:21Z"
  name: pod-reader
  namespace: default
  resourceVersion: "915"
  uid: 63911ac9-127d-4f39-b113-e94bacf4e692
rules:
- apiGroups:
  - ""
  resources:
  - pods
  verbs:
  - get
  - watch
  - list
```

kubectl get rolebinding read-pods -o yaml
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: "2024-03-12T09:08:21Z"
  name: read-pods
  namespace: default
  resourceVersion: "916"
  uid: 2ea4cd18-b1b0-47c5-9d9b-5d9828a96a30
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
subjects:
- kind: ServiceAccount
  name: dashboard-sa
  namespace: default
```





From kubernetes 1.24 onwards, whenever you create a serviceaccount it won't create the secret and token automatically.

If you want you can create the token manually using the below command

```
kubectl create token <service-account-name>
```

If you want to set/update the serviceaccount otherthan default you can do that by using below command (or) you can add 'serviceAccountName:<service-account-name>' in the deployment

```
kubectl set serviceaccount deploy/<deployment-name>
```





