
For this workshop, I have already downloaded the required Maria DB image from **VMware Application Catalog** and uploaded into Harbor Registry. 

<p style="color:blue"><strong> Review the Mariadb install file </strong></p>

```execute
cat $HOME/mariadb-install.yaml
```

<p style="color:blue"><strong> Review workload yaml file </strong></p>

```execute
cat $HOME/workload-springpetclinic.yaml
```

<p style="color:blue"><strong> Create MariaDB resources in tap-workload namespace </strong></p>

```execute
kubectl apply -f $HOME/mariadb-install.yaml -n tap-workload
```
<p style="color:blue"><strong> List the pods in tap-workload namespace </strong></p>

```execute
kubectl get pods -n tap-workload
```
<p style="color:blue"><strong> Get the deployed MariaDB pod name </strong></p>

```execute
dbpod=$(kubectl get pods -n tap-workload --no-headers=true | awk '/db/{print $1}' | xargs)
echo $dbpod
```

<p style="color:blue"><strong> Connect to DB pod </strong></p>

```execute
kubectl exec -it $dbpod -n tap-workload -- bash
```

<p style="color:blue"><strong> Connect to DB by providing the password as "secret" </strong></p>

```execute
mysql -h spring-petclinic-db -uroot -p
```

<p style="color:blue"><strong> Show the created databases </strong></p>

```execute
SHOW DATABASES;
```

<p style="color:blue"><strong> Connect to DB "test" </strong></p>

```execute
USE test;
```

<p style="color:blue"><strong> Show the tables and you wont find any at this point of time </strong></p>

```execute
SHOW TABLES;
```

![Local host](images/db-1.png)

<p style="color:blue"><strong> Grant required permissions for "test" DB </strong></p>

```execute
GRANT ALL PRIVILEGES ON *.* TO 'user'@'%' IDENTIFIED BY "pass";
```

```execute
\q
```

```execute
exit
```

<p style="color:blue"><strong> Create a workload using the config that is pre created for this workshop </strong></p>

```execute
tanzu apps workload apply -f $HOME/workload-springpetclinic.yaml --local-path $HOME/spring-petclinic-2.6.0-SNAPSHOT.jar --source-image harborairgap.tanzupartnerdemo.com/{{ session_namespace }}/spring-petclinic-source -n tap-workload --type web --label apps.tanzu.vmware.com/has-tests=true -y
```

<p style="color:blue"><strong> Check the live progress of application, it should take around 5-10 to complete </strong></p>

```execute
tanzu apps workload tail sbtest --since 10m --timestamp -n tap-workload
```

```execute
<ctrl+c>
```

<p style="color:blue"><strong> Check the deployed workload info and it should be in Ready state </strong></p>

```execute
tanzu apps workload get sbtest -n tap-workload
```

![Local host](images/sb-3.png)

Access the workload https://sbtest.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com {{copy}} from App Stream browser url

![Local host](images/sb-4.png)

<p style="color:blue"><strong> Connect to DB pod </strong></p>

```execute
kubectl exec -it $dbpod -n tap-workload -- bash
```

<p style="color:blue"><strong> Connect to DB, by providing the password as "secret" </strong></p>

```execute
mysql -h spring-petclinic-db -uroot -p
```

<p style="color:blue"><strong> Connect to DB "test" </strong></p>

```execute
USE test;
```

<p style="color:blue"><strong> You can see the tables listed now </strong></p>

```execute
SHOW TABLES;
```

<p style="color:blue"><strong> Query the owners, these are default values </strong></p>

```execute
select * from owners;
```

Now get back to App stream, url: https://sbtest.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com {{copy}} and add owner details: 

Find Owners > Add Owner > 

![Local host](images/sb-5.png)


![Local host](images/sb-6.png)

<p style="color:blue"><strong> Query the table and you can find the added owners in output </strong></p>

```execute
select * from owners;
```

![Local host](images/sb-7.png)

<p style="color:blue"><strong> Exit DB console </strong></p>

```execute
\q
```

<p style="color:blue"><strong> Exit DB pod bash console </strong></p>

```execute
exit
```
