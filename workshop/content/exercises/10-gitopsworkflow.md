

```execute
kubectl apply -f install-argocd.yaml -n argocd
```

```execute
kubectl annotate service argocd-server  -n argocd service.beta.kubernetes.io/aws-load-balancer-internal=true
```

```execute
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

```execute
kubectl create secret tls argocd-server-tls --cert=$HOME/tap-airgap-w01-s002.fullchain.pem  --key=$HOME/tap-airgap-w01-s002.privkey.pem -n argocd
```
