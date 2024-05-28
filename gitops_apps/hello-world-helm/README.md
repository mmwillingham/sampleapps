argocd app create demo-app \
--repo https://github.com/mmwillingham/sampleapps.git \
--path demo-app \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace demo-app \
--sync-option CreateNamespace=true \
--parameter namespace=demo-app

argocd app delete hello-world-helm -y

Delete from console until understand why "FATA[0000] rpc error: code = PermissionDenied desc = permission denied "
