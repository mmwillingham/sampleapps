```
argocd app create etherpad-prod \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/etherpad/prod \
--dest-server https://kubernetes.default.svc \
--sync-policy automated  \
--self-heal \
--sync-option Prune=true \
--dest-namespace etherpad-prod \
--sync-option CreateNamespace=true \
--grpc-web



# DELETE
argocd app delete etherpad-prod -y
```







```
<!-- argocd app create etherpad-prod \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/etherpad/prod \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace etherpad-prod \
--sync-option CreateNamespace=true \ -->
```

