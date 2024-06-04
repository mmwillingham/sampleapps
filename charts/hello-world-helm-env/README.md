```
argocd app create hello-world-helm-env \
--repo https://mmwillingham.github.io/sampleapps \
--helm-chart hello-world-helm-env \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace hello-world-helm-env \
--sync-option CreateNamespace=true \
--parameter namespace=hello-world-helm-env

# DELETE
argocd app delete hello-world-helm-env -y
```
