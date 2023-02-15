In this section, lets make some changes to the scanpolicy file and see how the application deployment goes through, how the errors can be fixed. 

```execute
tanzu apps workload list -n tap-workload
```

Edit scanpolicy and Add "Critical" to notAllowedSeverities list: 

```execute
vi $HOME/scanpolicy.yaml
```

ref Image: ![Scanpolicy](images/scanpolicy-1.png)

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-workload
```

```execute
tanzu apps workload create dev --git-repo https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo  --git-branch main --type web -n tap-workload --label apps.tanzu.vmware.com/has-tests=true --label app.kubernetes.io/part-of={{ session_namespace }} --param-yaml buildServiceBindings='[{"name": "settings-xml", "kind": "Secret"}, {"name": "ca-certificate", "kind": "Secret"}]' --build-env "BP_MAVEN_BUILD_ARGUMENTS=-debug -Dmaven.test.skip=true --no-transfer-progress package" -y
```

```execute
tanzu apps workload apply dev --annotation autoscaling.knative.dev/minScale=1 -n tap-workload -y
```

```execute
tanzu apps workload get dev -n tap-workload
```

```execute-2
tanzu apps workload tail dev -n tap-workload
```

```dashboard:open-url
https://tap-gui.{{ session_namespace }}.tap.tanzupartnerdemo.com/supply-chain/host/tap-workload/test
```

After few mins, you notice the workload deployment do not progress and few errors can be under workload supply chain in TAP GUI as shown below: 

ref Image: ![Scanpolicy](images/scan-1.png)

ctrl+c

Add the CVE **GHSA-36p3-wjmg-h94x** to ignoreCves list as shown in below image: 

ref Image: ![Scanpolicy](images/scan-2.png)

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-workload
```

```execute
tanzu apps workload get dev -n tap-workload
```

```dashboard:open-url
url: http://tap-gui.{{ session_namespace }}.demo.tanzupartnerdemo.com/supply-chain/host/tap-install/partnertapdemo-testscanpolicy
```

ref Image: ![Scanpolicy](images/scan-3.png)


ref Image: ![Scanpolicy](images/scan-5.png)

###### Verify in TAP GUI Supply chain status: 

```dashboard:open-url
url: http://tap-gui.{{ session_namespace }}.demo.tanzupartnerdemo.com/supply-chain/host/tap-install/partnertapdemo-testscanpolicy
```

ref Image: ![Scanpolicy](images/scan-6.png)

```execute
kubectl get svc envoy -n tanzu-system-ingress -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

```terminal:interrupt
session: 2
```

ref Image: ![Scanpolicy](images/scan-7.png)

```dashboard:open-url
url: http://partnertapdemo-testscanpolicy.tap-install.{{ session_namespace }}.demo.tanzupartnerdemo.com
```

ref Image: ![Scanpolicy](images/scan-8.png)
