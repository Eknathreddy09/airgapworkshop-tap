```execute
cat $HOME/reprosilite.yaml
```

```execute
kubectl create ns reposilite
```

```execute
kubectl create secret tls  my-tls-secret --cert=$HOME/fullchain.pem  --key=$HOME/privkey.pem -n reposilite
```

```execute
kubectl apply -f $HOME/reprosilite.yaml -n reposilite
```

```execute
kubectl get all -n reposilite
```

```execute
kubectl get pods -n reposilite

```execute
kubectl get svc -n tanzu-system-ingress
```

```execute-2
cd $HOME/tanzu-java-web-app

```execute-2
mvn --version

```execute-2
mvn package

create private hosted entry in route 53 pointing to the IP and 


