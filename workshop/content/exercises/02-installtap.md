
```execute-2
sed -i -r "s/SESSION_NAME/$SESSION_NAME/g" /home/eduk8s/tanzu-java-web-app/config/workload.yaml && sed -i -r "s/SESSION_NAME/$SESSION_NAME/g" /home/eduk8s/tanzu-java-web-app/catalog/catalog-info.yaml
```

```execute-2
scp -i tap-workshop.pem -r $HOME/tanzu-java-web-app/ $SESSION_NAME@10.0.1.62:/home/{{ session_namespace }}/ 
```

```execute
cd tanzu-java-web-app && git init && git add . && git commit -m "updated changes" && git remote remove origin && git remote add origin https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo.git && git push https://root:Newstart1@gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo HEAD:main --force
```

<p style="color:blue"><strong> The default maximum allowed built age of Grype’s vulnerability database is 5 days. This means that scanning with a 6 day old database causes the scan to fail. Below secret yaml uses the GRYPE_DB_MAX_ALLOWED_BUILT_AGE parameter to override the default value. </strong></p>

```execute
kubectl apply -f $HOME/secret-overwriteagegrype.yaml
```

<p style="color:blue"><strong> Review Tap values file </strong></p>

```execute
cd .. && cat $HOME/tap-values.yaml
```

<p style="color:blue"><strong> Install Tanzu package with full profile</strong></p>

```execute
tanzu package install tap -p tap.tanzu.vmware.com -v 1.4.0 --values-file $HOME/tap-values.yaml -n tap-install
```

![Local host](images/airgap-11.png)


Note: This process takes about 5-10 mins to complete. If you see any reconcile errors, please let us know.

<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
tanzu package installed list -A
```

![Local host](images/airgap-12.png)


<p style="color:red"><strong> Proceeed further only once all the packages are reconciled successfully </strong></p>


<p style="color:blue"><strong> Install Tanzu build service - full dependency </strong></p>

```execute
tanzu package install full-tbs-deps -p full-tbs-deps.tanzu.vmware.com -v 1.9.0 -n tap-install
```

![Local host](images/airgap-13.png)


<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
tanzu package installed list -A
```

<p style="color:blue"><strong> List the pods in build service namespace </strong></p>

```execute
kubectl get pods -n build-service
```

![Local host](images/airgap-14.png)

