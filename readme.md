# ArgoCD tutorial


## Start minikube
```
minikube start
```

## Installing latest/stable version of ArgoCD
```
# source:  https://argo-cd.readthedocs.io/en/stable/getting_started/
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## ArgoCD Login via UI
```
### Forward Ports
k get services -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:443
# You can now access argoCD on localhost:8080

### Get Credentials
# Username: admin
# password: see below
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## ArgoCD Login via ArgoCD-CLI 
```
# Install argoCD CLI --> 
# Port forward (if not done above)
kubectl port-forward svc/argocd-server -n argocd 8080:443
## Login to argoCD deployment (will require credentials as above)
argocd login 127.0.0.1:8080
```

### Creating an Application using ArgoCD CLI:
```
argocd app create webapp-kustom-prod \
--repo https://github.com/olivier-2018/argocd_tutorial \
--path kustom-webapp/overlays/prod \
--dest-server https://kubernetes.default.svc \
--dest-namespace prod

argocd app sync argocd/webapp-kustom-prod 

k create namespace prod 
```

### ArgoCD CLI Command Cheat sheet
```
argocd app create #Create a new Argo CD application.
argocd app list #List all applications in Argo CD.
argocd app logs <appname> #Get the application’s log output.
argocd app get <appname> #Get information about an Argo CD application.
argocd app diff <appname> #Compare the application’s configuration to its source repository.
argocd app sync <appname> #Synchronize the application with its source repository.
argocd app history <appname> #Get information about an Argo CD application.
argocd app rollback <appname> #Rollback to a previous version
argocd app set <appname> #Set the application’s configuration.
argocd app delete <appname> #Delete an Argo CD application.
```

## Accessing App on browser
```
# Set contect to dev namespace (if required)
kubectl config set-context minikube --namespace dev
# Get all resources to identify service name
kubectl get all
# Port forward of app service (assuming that container app is listening on port 80)
kubectl port-forward service/kustom-mywebapp-v1 -n dev 3000:80
# Accessing app on browser --> localhost:3000
```

## Removing apps and closing minikube
```
argocd app delete <appname> 
minikube stop
```

## Thanks:

https://youtu.be/JLrR9RV9AFA





