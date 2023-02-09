<p style="color:blue"><strong> Review below yaml files </strong></p>

```execute
cat $HOME/developer.yaml
```

```execute
cat $HOME/settings-xml.yaml
```

```execute
cat $HOME/scanpolicy.yaml
```

```execute
cat $HOME/tekton-pipeline.yaml
```

<p style="color:blue"><strong> Setup developer namespace </strong></p>

```execute
kubectl apply -f $HOME/developer.yaml -n tap-workload
```

```execute
kubectl apply -f $HOME/scanpolicy.yaml -n tap-workload
```

```execute
kubectl apply -f $HOME/tekton-pipeline.yaml -n tap-workload
```

```execute
kubectl apply -f $HOME/settings-xml.yaml -n tap-workload
```

```execute
kubectl apply -f $HOME/ca-certificate -n tap-workload
```

<p style="color:blue"><strong> List the packages installed </strong></p>

```execute
tanzu package installed list -A
```

* Open the file to edit the value of mvn wrapper properties 
```editor:select-matching-text
file: /home/eduk8s/tanzu-java-web-app/.mvn/wrapper/maven-wrapper.properties
text: "reposiliteairgap"
```

* Update the value of `distributionUrl` and `wrapperUrl`
```editor:replace-text-selection
file: /home/eduk8s/tanzu-java-web-app/.mvn/wrapper/maven-wrapper.properties
text: {{ session_namespace }}
```

* Open the file to edit the value of mvn wrapper properties 
```editor:select-matching-text
file: /home/eduk8s/tanzu-java-web-app/.mvn/wrapper/maven-wrapper.properties
text: "reposiliteairgap"
```

* Update the value of `distributionUrl` and `wrapperUrl`
```editor:replace-text-selection
file: /home/eduk8s/tanzu-java-web-app/.mvn/wrapper/maven-wrapper.properties
text: {{ session_namespace }}
```

* Open the file to edit the value of mvnw and point to reposilite maven repository
```editor:select-matching-text
file: /home/eduk8s/tanzu-java-web-app/mvnw
text: "reposiliteairgap"
```

* Update the value of `jarUrl` in mvnw
```editor:replace-text-selection
file: /home/eduk8s/tanzu-java-web-app/mvnw
text: {{ session_namespace }}
```

* Open the file to edit the value of `DOWNLOAD_URL` env variable in mvnw.cmd
```editor:select-matching-text
file: /home/eduk8s/tanzu-java-web-app/mvnw.cmd
text: "reposiliteairgap"
```

* Update the value of `DOWNLOAD_URL` in mvnw.cmd
```editor:replace-text-selection
file: /home/eduk8s/tanzu-java-web-app/mvnw.cmd
text: {{ session_namespace }}
```

* Open the file to edit the value of `DOWNLOAD_URL` env variable
```editor:select-matching-text
file: /home/eduk8s/tanzu-java-web-app/mvnw.cmd
text: "reposiliteairgap"
```

* Update the value of `DOWNLOAD_URL`
```editor:replace-text-selection
file: /home/eduk8s/tanzu-java-web-app/mvnw.cmd
text: {{ session_namespace }}
```

```execute-2
scp -i $HOME/tap-workshop.pem -r $HOME/tanzu-java-web-app/ {{ session_namespace }}@10.0.1.62:/home/{{ session_namespace }}/
```

##### Deploy a workload (app) using local path: 

```execute
tanzu apps workload create app --local-path tanzu-java-web-app/ --type web -n tap-workload --source-image harborairgap.tanzupartnerdemo.com/{{ session_namespace }}/build-service/{{ session_namespace }}-source-new --param-yaml buildServiceBindings='[{"name": "settings-xml", "kind": "Secret"}, {"name": "ca-certificate", "kind": "Secret"}]' --build-env "BP_MAVEN_BUILD_ARGUMENTS=-debug -Dmaven.test.skip=true --no-transfer-progress package" -y
```

![Local host](images/airgap-19.png)


<p style="color:blue"><strong> Get the status of deployed application </strong></p>

```execute
tanzu apps workload get app -n tap-workload
```

![Local host](images/airgap-20.png)

<p style="color:blue"><strong> Check the live progress of application </strong></p>

```execute-1
tanzu apps workload tail app --since 10m --timestamp -n tap-workload
```

![Local host](images/airgap-81.png)

```execute-1
<ctrl+c>
```

<p style="color:blue"><strong> Check all the installed applications </strong></p>

```execute
tanzu apps workload list -n tap-workload
```

![Local host](images/airgap-89.png)

###### Apply Annotation

```execute
tanzu apps workload apply app --annotation autoscaling.knative.dev/minScale=1 -n tap-workload -y
```

![Local host](images/airgap-90.png)

###### Note: Workload creation takes 5 mins to complete, proceed further once you see ready status

```execute
tanzu apps workload get app -n tap-workload
```

![Workload](images/airgap-82.png)

<p style="color:blue"><strong> Get the pods in tap-install namespace </strong></p>

```execute
kubectl get pods -n tap-workload
```

![Workload](images/airgap-83.png)

<p style="color:blue"><strong> Access the deployed application in App Stream browser- https://app.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com</strong></p>

![Workload](images/airgap-92.png)

### Workload pointing to Git Repository

###### Upload the files to Gitlab project

```execute
cd tanzu-java-web-app && git init && git add . && git commit -m "updated changes" && git remote remove origin && git remote add origin https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo.git && git push https://root:Newstart1@gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo HEAD:main --force
```

##### Create a secret using Flux cli

```execute
cd .. && flux create secret git git-secret -u root -p Newstart1 --ca-file=gitea.crt --url=https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo.git -n tap-workload
```

##### Deploy a workload ({{ session_namespace }}-git) by pointing to Gitlab project: 

```execute
tanzu apps workload create gitapp --git-repo https://gitlab.tap.tanzupartnerdemo.com/gitlab-instance-081097ef/$SESSION_NAME-repo  --git-branch main --type web -n tap-workload --label apps.tanzu.vmware.com/has-tests=true --label app.kubernetes.io/part-of={{ session_namespace }} --param-yaml buildServiceBindings='[{"name": "settings-xml", "kind": "Secret"}, {"name": "ca-certificate", "kind": "Secret"}]' --build-env "BP_MAVEN_BUILD_ARGUMENTS=-debug -Dmaven.test.skip=true --no-transfer-progress package" -y
```

```execute
tanzu apps workload list -n tap-workload
```

```execute
tanzu apps workload tail gitapp --namespace tap-workload
```

![Workload](images/airgap-94.png)

```execute-1
<ctrl+c>
```

```execute
tanzu apps workload get gitapp -n tap-workload
```

![Workload](images/airgap-93.png)

<p style="color:blue"><strong> Access the deployed application in App Stream browser- https://gitapp.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com</strong></p>

![Workload](images/airgap-96.png)

### Pre-build image: 

```dashboard:open-url
url: https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/scc-pre-built-image.html
```

```execute
tanzu apps workload list -n tap-workload
```

Note: Image is already created for this workshop and uploaded to Harbor Registry. 

```execute
tanzu apps workload create fromimage --image harborairgap.tanzupartnerdemo.com/tapairgap/prebuildimage:latest --type web --app {{ session_namespace }}-fromimage -n tap-workload -y
```

```execute-1
tanzu apps workload tail fromimage --namespace tap-workload
```

```execute-1
<ctrl+c>
```

```execute
tanzu apps workload get fromimage -n tap-workload
```

![Workload from Image](images/airgap-97.png)


<p style="color:blue"><strong> Access the deployed application in App Stream browser- https://fromimage.tap-workload.{{ session_namespace }}.tap.tanzupartnerdemo.com </strong></p>

![Application Access](images/airgap-99.png)
