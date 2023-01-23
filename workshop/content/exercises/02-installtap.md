<p style="color:blue"><strong> Review Tap values file </strong></p>

```execute
cat $HOME/tap-values.yaml
```

<p style="color:blue"><strong> Install Tanzu package with full profile</strong></p>

```execute
sudo tanzu package install tap -p tap.tanzu.vmware.com -v 1.4.0 --values-file $HOME/tap-values.yaml -n tap-install
```

![Install](images/install-3.png)

Note: This process takes about 5-10 mins to complete. If you see any reconcile errors, please let us know.

After successful installation: ![Install](images/install-4.png)

<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
sudo tanzu package installed list -A
```

<p style="color:red"><strong> Proceeed further only once all the packages are reconciled successfully </strong></p>

![Install](images/install-5.png)
