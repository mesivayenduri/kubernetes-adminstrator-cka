# Different types of authorization mechanisms
- Node
- ABAC
- RBAC
- Webhook
- AlWaysAllow
- AlWaysDeny


* We can set modes using authorization-mode option on the kube-apiserver.
* We can set multiple modes using comma seperated values

* Default modes is "AlwaysAllow"



# RBAC ( Role Based Access Controls )
- Role
- RoleBinding


# Check access if you have access to resources

```kubectl auth can-i create deployments```

# If you are a admin, and you want to impersonate and check access of a person to a specific resource

```kubectl auth can-i create pods --as dev-user```




# Challenge: 

Create the necessary roles and role bindings required for the dev-user to create, list and delete pods in the default namespace.
Use the given requirements
                            Role: developer
                            Role Resources: pods
                            Role Actions: list
                            Role Actions: create
                            Role Actions: delete
                            RoleBinding: dev-user-binding
                            RoleBinding: Bound to dev-user

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["list", "create", "delete"]
```
or

```
kubectl create role developer --verb=list,create,delete --resoruce=pods
```


```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dev-user-binding
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```
or

```
kubectl create rolebinding dev-user-binding --role=developer --user=dev-user
```