In this section, lets make some changes to the scanpolicy file and see how the application deployment goes through, how the errors can be fixed. 

```execute
sudo tanzu apps workload list -n tap-install
```

Edit scanpolicy and Add "Critical" to notAllowedSeverities list: 

```editor:open-file
file: /home/eduk8s/scanpolicy.yaml
line: 17
```

ref Image: ![Scanpolicy](images/scanpolicy-1.png)

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-install
```

```execute
sudo tanzu apps workload create partnertapdemo-testscanpolicy  --git-repo https://github.com/Eknathreddy09/partnertapdemo-app --git-branch main --type web --label apps.tanzu.vmware.com/has-tests=true --label app.kubernetes.io/part-of=partnertapdemo -n tap-install --yes
```

```execute
tanzu apps workload apply partnertapdemo-testscanpolicy --annotation autoscaling.knative.dev/minScale=1 -n tap-install -y
```

```execute
sudo tanzu apps workload get partnertapdemo-testscanpolicy -n tap-install
```

```execute-2
sudo tanzu apps workload tail partnertapdemo-testscanpolicy -n tap-install
```

```dashboard:open-url
url: http://tap-gui.{{ session_namespace }}.demo.tanzupartnerdemo.com/supply-chain/host/tap-install/partnertapdemo-testscanpolicy
```

After few mins, you notice the workload deployment do not progress and few errors can be under workload supply chain in TAP GUI as shown below: 

ref Image: ![Scanpolicy](images/scan-1.png)

```editor:open-file
file: /home/eduk8s/scanpolicy.yaml
line: 18
```

Add the CVE **GHSA-36p3-wjmg-h94x** to ignoreCves list as shown in below image: 

ref Image: ![Scanpolicy](images/scan-2.png)

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-install
```

Soon after the scanpolicy is updated, you can see the deployment progressing in **Terminal-2**. Wait for a min and it will pause again

```execute
sudo tanzu apps workload get partnertapdemo-testscanpolicy -n tap-install
```

```dashboard:open-url
url: http://tap-gui.{{ session_namespace }}.demo.tanzupartnerdemo.com/supply-chain/host/tap-install/partnertapdemo-testscanpolicy
```

ref Image: ![Scanpolicy](images/scan-3.png)

Add the CVE **CVE-2022-22965** to ignoreCves list as shown in below image: 

```editor:open-file
file: /home/eduk8s/scanpolicy.yaml
line: 18
```

ref Image: ![Scanpolicy](images/scan-4.png)

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-install
```

After updating the scanpolicy, you can see the progress of application going through

```execute
sudo tanzu apps workload get partnertapdemo-testscanpolicy -n tap-install
```

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

Add an entry in local host /etc/hosts path pointing the above collected *load balancer IP* with **partnertapdemo-testscanpolicy.tap-install.{{ session_namespace }}.demo.tanzupartnerdemo.com**

ref Image: ![Scanpolicy](images/scan-7.png)

```dashboard:open-url
url: http://partnertapdemo-testscanpolicy.tap-install.{{ session_namespace }}.demo.tanzupartnerdemo.com
```

ref Image: ![Scanpolicy](images/scan-8.png)
