
<p style="color:blue"><strong> Click here to test the execution in terminal</strong></p>

```execute-all
echo "Hello, Welcome to Partner workshop session"
```

<p style="color:blue"><strong> Set environment variable </strong></p>

```execute-all
export SESSION_NAME={{ session_namespace }}
```

<p style="color:blue"><strong> Click here to test the execution in terminal</strong></p>

```execute-1
ssh -i tap-workshop.pem $SESSION_NAME@10.0.1.62 -o StrictHostKeyChecking=accept-new
```

<p style="color:blue"><strong> Click here to check the Tanzu version</strong></p>

```execute-1
tanzu version
```

<p style="color:blue"><strong> Click here to check the AWS CLI version</strong></p>

```execute
aws --version
```

<p style="color:blue"><strong> Click here to check the kubectl version</strong></p>

```execute
kubectl version
```

Note: Since we are deploying TAP on TKGm cluster, cluster essentials is not being installed. If you are installing TAP on any other K8s cluster, the follow the steps in https://docs.vmware.com/en/Cluster-Essentials-for-VMware-Tanzu/1.4/cluster-essentials/deploy.html for installing Cluster essentials. 

<p style="color:blue"><strong> Set the context</strong></p>

```execute
kubectl config use-context {{ session_namespace }}-admin@{{ session_namespace }}
```

<p style="color:blue"><strong> Check if the current context is set to "{{ session_namespace }}"</strong></p>

```execute
kubectl config get-contexts
```

```execute-all
export IMGPKG_REGISTRY_USERNAME=admin
```

```execute-all
export IMGPKG_REGISTRY_PASSWORD=Harbor12345
```

```execute-all
docker login harborairgap.tanzupartnerdemo.com -u $IMGPKG_REGISTRY_USERNAME -p $IMGPKG_REGISTRY_PASSWORD
```

```execute-2
docker login registry.tanzu.vmware.com
```

```execute-all
export IMGPKG_REGISTRY_HOSTNAME=harborairgap.tanzupartnerdemo.com/$SESSION_NAME
```

```execute-all
export TAP_VERSION=1.4.0
```

```execute-1
export REGISTRY_CA_PATH=/home/tap-airgap-w01-s001/harborairgap.tanzupartnerdemo.com.crt
```

```execute-1
export cadata=$(yq  '.clusters[] | select (.name == "{{ session_namespace }}") | .cluster.certificate-authority-data' /home/tap-airgap-w01-s001/config)
```

```execute-2
imgpkg copy -b registry.tanzu.vmware.com/tanzu-application-platform/tap-packages:$TAP_VERSION --to-tar $HOME/tap-packages-$TAP_VERSION.tar --include-non-distributable-layers
```

```execute-2
imgpkg copy -b registry.tanzu.vmware.com/tanzu-application-platform/full-tbs-deps-package-repo:1.9.0 --to-tar=$HOME/tbs-full-deps.tar
```

```execute-2
scp -i tap-workshop.pem $HOME/tap-packages-$TAP_VERSION.tar $SESSION_NAME@10.0.1.62:$HOME
```

```execute-2
scp -i tap-workshop.pem $HOME/tbs-full-deps.tar $SESSION_NAME@10.0.1.62:$HOME
```

```execute-1
ls -ltrh | grep "$HOME/tap-packages-$TAP_VERSION.tar"
```

```execute-1
ls -ltrh | grep "$HOME/tbs-full-deps.tar"
```

```execute-1
imgpkg copy --tar $HOME/tap-packages-$TAP_VERSION.tar --to-repo $IMGPKG_REGISTRY_HOSTNAME/tap-packages --include-non-distributable-layers --registry-ca-cert-path $REGISTRY_CA_PATH
```

```execute-1
imgpkg copy --tar $HOME/tbs-full-deps.tar --to-repo=$IMGPKG_REGISTRY_HOSTNAME/tbs-full-deps --registry-ca-cert-path $REGISTRY_CA_PATH
```

<p style="color:blue"><strong> Create a namespace </strong></p>

```execute
kubectl create ns tap-install
```

```execute
tanzu secret registry add tap-registry --server   $IMGPKG_REGISTRY_HOSTNAME --username $IMGPKG_REGISTRY_USERNAME --password $IMGPKG_REGISTRY_PASSWORD --namespace tap-install --export-to-all-namespaces --yes
```

<p style="color:blue"><strong> Add the package repository </strong></p>

```execute
tanzu package repository add tanzu-tap-repository --url $IMGPKG_REGISTRY_HOSTNAME/tap-packages:$TAP_VERSION --namespace tap-install
```

```execute
tanzu package repository add tbs-full-deps-repository --url $IMGPKG_REGISTRY_HOSTNAME/tbs-full-deps:1.9.0 --namespace tap-install
```

<p style="color:blue"><strong> Get the status of the TAP package repository, and ensure the status updates to Reconcile succeeded </strong></p>

```execute
tanzu package repository get tanzu-tap-repository --namespace tap-install
```

<p style="color:blue"><strong>  List the available packages </strong></p>

```execute
tanzu package available list --namespace tap-install
```

```execute
tanzu package available list tap.tanzu.vmware.com --namespace tap-install
```

```
tanzu secret registry add registry-credentials --server   $IMGPKG_REGISTRY_HOSTNAME --username $IMGPKG_REGISTRY_USERNAME --password $IMGPKG_REGISTRY_PASSWORD --namespace tap-install --export-to-all-namespaces --yes
```

```execute
echo "YWRtaW4tYWlyZ2FwCg==" | base64 -d
```

```execute
echo "V2VsY29tZTExIQo=" | base64 -d
```

```execute
kubectl create secret generic git-secret --from-literal=username="YWRtaW4tYWlyZ2FwCg==" --from-literal=password="V2VsY29tZTExIQo=" -n tap-install
```


<p style="color:blue"><strong> Changes to tap values file" </strong></p>

```execute
sed -i -r "s/SESSION_NAME/$SESSION_NAME/g" $HOME/tap-values.yaml
```

```execute
sed -i -r "s/providecadata/$cadata/g" $HOME/tap-values.yaml
```

```execute
sed -i -r "s/reposiliteairgap/$SESSION_NAME/g" $HOME/reprosilite.yaml
```
