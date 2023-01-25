<p style="color:blue"><strong> Review Tap values file </strong></p>

```execute
cat $HOME/tap-values.yaml
```

<p style="color:blue"><strong> Install Tanzu package with full profile</strong></p>

```execute
tanzu package install tap -p tap.tanzu.vmware.com -v 1.4.0 --values-file $HOME/tap-values.yaml -n tap-install
```

Note: This process takes about 5-10 mins to complete. If you see any reconcile errors, please let us know.

<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
tanzu package installed list -A
```

<p style="color:red"><strong> Proceeed further only once all the packages are reconciled successfully </strong></p>


<p style="color:blue"><strong> Install Tanzu build service - full dependency </strong></p>

```execute
tanzu package install full-tbs-deps -p full-tbs-deps.tanzu.vmware.com -v 1.9.0 -n tap-install
```

<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
tanzu package installed list -A
```

<p style="color:blue"><strong> List the pods in build service namespace </strong></p>

```execute
kubectl get pods -n build-service
```
