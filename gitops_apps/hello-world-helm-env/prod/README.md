```
argocd app create hello-world-helm-env-prod \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world-helm-env/prod \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace hello-world-helm-env-prod \
--sync-option CreateNamespace=true \
--parameter namespace=hello-world-helm-env-prod

# DELETE
argocd app delete hello-world-helm-env-prod -y
```
