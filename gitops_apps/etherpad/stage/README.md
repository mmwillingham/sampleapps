argocd app create etherpad-stage \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/etherpad/stage \
--dest-server https://kubernetes.default.svc \
--sync-policy automated  \
--grpc-web

# DELETE
argocd app delete etherpad-stage -y
```