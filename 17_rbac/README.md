kubectl auth can-i get pod

kubectl auth whoami

kubectl auth can-i get pod --as adam

it says no, we to give the user permission we need to create role and rolebinding

then we write role.yaml file with necessary permissions and apply if

kubectl apply -f role.yaml

kubectl get role

kubectl describe role <role-name>

then we need to bind this role

create binding.yaml

kubectl apply -f binding.yaml

kubectl get rolebinding

kubectl describe rolebinding <RoleBinding-name>

kubectl auth can-i get pod --as adam
now you will get --> yes

get all roles
kubectl get roles -A

count number of roles
kubectl get roles -A --no-headers | wc -l

for windose
kubectl get roles -A --no-headers | Measure-Object -Line

add entry in kubeconfig:

```
kubectl config set-credentials adam \
--client-key = ../15_manage_tls/adam.key \
--client-certificate=../15_manage_tls/adam.crt \
--embed-certs=true
```

```Powershell
kubectl config set-credentials adam `
--client-key=..\15_manage_tls\adam.key `
--client-certificate=..\15_manage_tls\adam.crt `
--embed-certs=true
```

set context

```
kubectl config set-context adam --cluster=kind-cka-cluster1 --user=adam
```

check

```
kubectl config get-contexts
```

switch to adam

```
kubectl config use-context adam
```
