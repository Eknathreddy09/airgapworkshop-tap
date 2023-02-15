###### Delete the accelerator 

```execute
tanzu accelerator delete {{ session_namespace }}
```

###### List the workloads

```execute
tanzu apps workload list -n tap-workload
```

###### Delete the workload git

```execute
tanzu apps workload delete git -n tap-workload -y
```

```execute
rm *.pom && rm *.zip && rm *.tar && kubectl delete ns reposilite
```

###### Delete the TAP Package, this process takes around 3-4 mins to complete. 

```execute
tanzu package installed delete tap -n tap-install -y
```

```execute
exit
```

Note: Click on **END WORKSHOP** to close the session. 
