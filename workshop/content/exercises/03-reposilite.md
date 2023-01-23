<p style="color:blue"><strong> Review reprosilite yaml file </strong></p>

```execute
cat $HOME/reprosilite.yaml
```

<p style="color:blue"><strong> Create namespace reposilite </strong></p>

```execute
kubectl create ns reposilite
```

<p style="color:blue"><strong> Create secret using the pem files already saved in JB </strong></p>

```execute
kubectl create secret tls  my-tls-secret --cert=$HOME/fullchain.pem  --key=$HOME/privkey.pem -n reposilite
```

<p style="color:blue"><strong> Apply reprosilite yaml file in namespace reposilite </strong></p>

```execute
kubectl apply -f $HOME/reprosilite.yaml -n reposilite
```

<p style="color:blue"><strong> Check the pods in namespace reposilite </strong></p>

```execute
kubectl get all -n reposilite
```

<p style="color:blue"><strong> Check the load balancer </strong></p>

```execute
kubectl get svc -n tanzu-system-ingress
```

```execute-2
cd $HOME/tanzu-java-web-app
```

<p style="color:blue"><strong> Verify the maven version </strong></p>

```execute-2
mvn --version
```

<p style="color:blue"><strong> Build maven locally </strong></p>

```execute-2
mvn package
```

###### create private hosted entry in route 53 pointing to the IP

