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
loadbalancer=$(kubectl get svc envoy -n tanzu-system-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo $loadbalancer
```

#### Note: Provide the loadbalancer hostname and {{ session_namespace }} to SE in chat. 

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

```execute-2
scp -i ~/tap-workshop.pem -r /root/.m2/ $SESSION_NAME@jb-internetrestricted.tanzupartnerdemo.com:/home/$SESSION_NAME
```

```execute-2
scp -i ~/tap-workshop.pem $HOME/files/*.pom $SESSION_NAME@jb-internetrestricted.tanzupartnerdemo.com:/home/$SESSION_NAME
```

```execute-2
scp -i ~/tap-workshop.pem $HOME/files/*.zip $SESSION_NAME@jb-internetrestricted.tanzupartnerdemo.com:/home/$SESSION_NAME
```

```execute-2
scp -i ~/tap-workshop.pem $HOME/files/*.jar $SESSION_NAME@jb-internetrestricted.tanzupartnerdemo.com:/home/$SESSION_NAME
```

```execute
reposilitepod=$(kubectl get pods -n reposilite -o=jsonpath="{.items[*]['metadata.name', 'status.phase=Running']}")
```

```execute
kubectl cp .m2/repository/ $reposilitepod:/app/data/repositories/releases -n reposilite
```

```execute
kubectl cp spring-boot-starter-parent-2.5.8.pom $reposilitepod:/app/data/repositories/releases -n reposilite
```

```execute
kubectl cp spring-boot-dependencies-2.5.8.pom $reposilitepod:/app/data/repositories/releases -n reposilite
```

```execute
kubectl cp apache-maven-3.6.3-bin.zip $reposilitepod:/app/data/repositories/releases -n reposilite
```

```execute
kubectl cp maven-wrapper-0.5.6.jar $reposilitepod:/app/data/repositories/releases -n reposilite
```

```execute
kubectl exec -it $reposilitepod -n reposilite -- bash
```

```execute
cd data/repositories/releases
```

```execute
mv repository/* .
```

```execute
rm repository -r
```

```execute
mkdir -p org/springframework/boot/spring-boot-dependencies/2.5.8/
```

```execute
mv spring-boot-dependencies-2.5.8.pom org/springframework/boot/spring-boot-dependencies/2.5.8/
```

```execute
mkdir -p org/springframework/boot/spring-boot-starter-parent/2.5.8/
```

```execute
mv spring-boot-starter-parent-2.5.8.pom org/springframework/boot/spring-boot-starter-parent/2.5.8/
```

```execute
mkdir -p org/apache/maven/wrapper/0.5.6/
```

```execute
mv maven-wrapper-0.5.6.jar  org/apache/maven/wrapper/0.5.6/
```

```execute
mkdir -p org/apache/maven/apache-maven/3.6.3/
```

```execute
mv apache-maven-3.6.3-bin.zip  org/apache/maven/apache-maven/3.6.3/
```

```execute
exit
```

In windows JB, open google chrome and access the url https://{{ session_namespace }}.tap.tanzupartnerdemo.com/ and verify the copied files under releases as shown below: 
