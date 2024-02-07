# RestoreOnKubernetes

This repository demonstrates setting up and running .NET 8 ASP.NET App in Kubernetes

## Kubernetes Dashboard

Start the Kubernetes proxy:

```powershell
kubectl proxy
```

Access the Dashboard by opening the following URL in your web browser:

```text
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

Setup Login User w/token

Create a user to log into Kubernetes Dashboard > https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

Instructions for creating a sample user
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

```powershell
# create dashboard-adminuser.yaml
kubectl apply -f dashboard-adminuser.yaml
serviceaccount/admin-user created

# create dashboard-cluster-admin-role.yaml
kubectl apply -f dashboard-cluster-admin-role.yaml
clusterrolebinding.rbac.authorization.k8s.io/admin-user created
```

Get the Bearer Token

```powershell
kubectl -n kubernetes-dashboard create token admin-user
```

## Kubernetes Commands

```powershell
# create deployment
kubectl apply -f restore-deployment.yaml
# delete deployment
kubectl delete deployment restore-deployment

# delete service
kubectl delete service restore-service


```

### Docker Commands

```powershell
# build image
docker build -t restoreonkubernetes:test -f ./RestoreOnKubernetes/Dockerfile .
# run image
docker run --rm -d -p 8080:8080/tcp -p 8081:8081/tcp restoreonkubernetes:test

# remove old docker image
docker rmi jpcassidy/restoreonkubernetes:v1.0
# tag new image
docker tag restoreonkubernetes:test jpcassidy/restoreonkubernetes:v1.0

# push image to docker regstry
docker push jpcassidy/restoreonkubernetes:v1.0


```
