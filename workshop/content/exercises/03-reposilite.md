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

```execute-2
scp -i ~/tap-workshop.pem -r /root/.m2/ $SESSION_NAME@10.0.1.62:/home/$SESSION_NAME
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
mkdir -p org/springframework/boot/spring-boot-dependencies/2.6.9/
```

```execute
mv spring-boot-dependencies-2.6.9.pom org/springframework/boot/spring-boot-dependencies/2.6.9/
```

```execute
mkdir -p org/springframework/boot/spring-boot-dependencies/2.7.1/
```

```execute
mv spring-boot-dependencies-2.7.1.pom org/springframework/boot/spring-boot-dependencies/2.7.1/
```

```execute
mkdir -p org/springframework/boot/spring-boot-dependencies/2.7.3/
```

```execute
mv spring-boot-dependencies-2.7.3.pom org/springframework/boot/spring-boot-dependencies/2.7.3/
```

```execute
mkdir -p org/springframework/boot/spring-boot-starter-parent/2.5.8/
```

```execute
mv spring-boot-starter-parent-2.5.8.pom org/springframework/boot/spring-boot-starter-parent/2.5.8/
```

```execute
mkdir -p org/springframework/boot/spring-boot-starter-parent/2.6.9/
```

```execute
mv spring-boot-starter-parent-2.6.9.pom org/springframework/boot/spring-boot-starter-parent/2.6.9/
```

```execute
mkdir -p org/springframework/boot/spring-boot-starter-parent/2.5.8/
```

```execute
mv spring-boot-starter-parent-2.7.1.pom org/springframework/boot/spring-boot-starter-parent/2.7.1/
```

```execute
mkdir -p org/springframework/boot/spring-boot-starter-parent/2.7.3/
```

```execute
mv spring-boot-starter-parent-2.7.3.pom org/springframework/boot/spring-boot-starter-parent/2.7.3/
```

```execute
mkdir -p org/apache/maven/wrapper/0.5.6/
```

```execute
mv maven-wrapper-0.5.6.jar  org/apache/maven/wrapper/0.5.6/
```

```execute
mkdir -p org/apache/maven/wrapper/3.1.1/
```

```execute
mv maven-wrapper-3.1.1.jar  org/apache/maven/wrapper/3.1.1/
```

```execute
mkdir -p org/apache/maven/apache-maven/3.6.3/
```

```execute
mv apache-maven-3.6.3-bin.zip  org/apache/maven/apache-maven/3.6.3/
```

```execute
mkdir -p org/apache/maven/apache-maven/3.8.1/
```

```execute
mv apache-maven-3.8.1-bin.zip  org/apache/maven/apache-maven/3.8.1/
```

```execute
mkdir -p org/apache/maven/apache-maven/3.8.2/
```

```execute
mv apache-maven-3.8.2-bin.zip  org/apache/maven/apache-maven/3.8.2/
```

```execute
mkdir -p org/apache/maven/apache-maven/3.8.6/
```

```execute
mv apache-maven-3.8.6-bin.zip  org/apache/maven/apache-maven/3.8.6/
```
