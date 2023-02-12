
<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
kubectl get pods -n metadata-store
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
kubectl describe secret app-secret -n metadata-store
```

<p style="color:blue"><strong> Connect to the Postgres using the password "Welcome11!"</strong></p>

```execute
dbname=$(cut -c 16-19 <<< '{{ session_namespace }}') && psql --host=tap-workshop-db.cnw4j3rlp4eh.us-west-2.rds.amazonaws.com --port=5432 --username=postgres --dbname=$dbname 
```

<p style="color:blue"><strong> Read the config from lines ( 350-384 ) of tap-values.yaml, located in home directory
 </strong></p>

```execute
cat $HOME/tap-values.yaml
```

<p style="color:blue"><strong> List the databases</strong></p>

```execute
\l
```

<p style="color:blue"><strong> Check for the tables </strong></p>

```execute
\dt
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
select id,created_at,cve_id,url,cna from vulnerabilities;
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
select * from package_vulnerabilities;
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
select * from packages limit 10;
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
select * from package_vulnerabilities limit 15;
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
select * from ratings WHERE score > 5 LIMIT 10;
```

<p style="color:blue"><strong> retrieve the access token and store it in variable</strong></p>

```execute
\q
```
