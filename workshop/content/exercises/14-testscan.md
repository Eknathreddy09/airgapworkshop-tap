In this section, lets make some changes to the scanpolicy file and see how the application deployment goes through, how the errors can be fixed. 

```execute
tanzu apps workload list -n tap-workload
```

Edit scanpolicy and Add "Critical" to notAllowedSeverities list: 

```execute
vi $HOME/scanpolicy.yaml
```

ref Image: ![Scanpolicy](images/scanpolicy-1.png)

<p style="color:blue"><strong> Update the scanpolicy </strong></p>

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-workload
```

<p style="color:blue"><strong> Create a workload </strong></p>

```execute
tanzu apps workload create dev --git-repo https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo  --git-branch main --type web -n tap-workload --label apps.tanzu.vmware.com/has-tests=true --label app.kubernetes.io/part-of={{ session_namespace }} --param-yaml buildServiceBindings='[{"name": "settings-xml", "kind": "Secret"}, {"name": "ca-certificate", "kind": "Secret"}]' --build-env "BP_MAVEN_BUILD_ARGUMENTS=-debug -Dmaven.test.skip=true --no-transfer-progress package" -y
```

<p style="color:blue"><strong> Get the workload details </strong></p>

```execute
tanzu apps workload get dev -n tap-workload
```

<p style="color:blue"><strong> Check the live progress of workload deployment </strong></p>

```execute
tanzu apps workload tail dev --namespace tap-workload --timestamp --since 5m
```

Access the url **https://tap-gui.{{ session_namespace }}.tap.tanzupartnerdemo.com/supply-chain/host/tap-workload/dev** {{ copy }} in App Stream chrome browser

After few mins, you notice the workload deployment do not progress and few errors can be under workload supply chain in TAP GUI as shown below: 

ref Image: ![Scanpolicy](images/scan-1.png)

```execute
ctrl+c
```

Add the CVE **GHSA-36p3-wjmg-h94x** to ignoreCves list in scanpolicy as shown in below image: 

ref Image: ![Scanpolicy](images/scan-2.png)

<p style="color:blue"><strong> Update the scanpolicy </strong></p>

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-workload
```

<p style="color:blue"><strong> Check the live progress of workload deployment </strong></p>

```execute
tanzu apps workload tail dev --namespace tap-workload --timestamp --since 1h
```

```execute
<ctrl+c>
```

<p style="color:blue"><strong> Get the workload details </strong></p>

```execute
tanzu apps workload get dev -n tap-workload
```

ref Image: ![Scanpolicy](images/scan-3.png)

###### Verify the status of workload in TAP GUI Supply chain : 

Access the url **https://tap-gui.{{ session_namespace }}.tap.tanzupartnerdemo.com/supply-chain/host/tap-workload/dev** {{ copy }} in App Stream chrome browser

ref Image: ![Scanpolicy](images/scan-4.png)

Access the url **https://dev.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com/** {{ copy }} in App Stream chrome browser to access the dev application that is just deployed. 

ref Image: ![Scanpolicy](images/scan-5.png)
