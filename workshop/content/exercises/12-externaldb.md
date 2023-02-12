
<p style="color:blue"><strong> Check the metadata store pod status </strong></p>

```execute
kubectl get pods -n metadata-store
```

<p style="color:blue"><strong> Check the secret that contains the DB connection details</strong></p>

```execute
kubectl describe secret app-secret -n metadata-store
```

<p style="color:blue"><strong> Connect to the Postgres using the password - <p style="color:red"><strong> Welcome11! </strong></p> </strong></p>

```execute
dbname=$(cut -c 16-19 <<< '{{ session_namespace }}') && psql --host=tap-workshop-db.cnw4j3rlp4eh.us-west-2.rds.amazonaws.com --port=5432 --username=postgres --dbname=$dbname 
```

<p style="color:blue"><strong> Read the Metadata store config from lines ( 350-384 ) of tap-values.yaml, located in home directory
 </strong></p>

```execute
cat $HOME/tap-values.yaml
```

<p style="color:blue"><strong> List the databases </strong></p>

```execute
\l
```

<p style="color:blue"><strong> Check for the tables in connected database</strong></p>

```execute
\dt
```

<p style="color:blue"><strong> Check the vulnerabilities details  </strong></p>

```execute
select id,created_at,cve_id,url,cna from vulnerabilities limit 25;
```

<p style="color:blue"><strong> Check the package details </strong></p>

```execute
select * from packages limit 10;
```

<p style="color:blue"><strong> Check the package vulnerabilities details </strong></p>

```execute
select * from package_vulnerabilities limit 15;
```

<p style="color:blue"><strong> Check the Ratings table </strong></p>

```execute
select * from ratings WHERE score > 5 LIMIT 10;
```

Note: You can explore the tables by using custom queries for few minutes before exiting the DB. 

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
\q
```
