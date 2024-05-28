argocd app create hello-world-helm \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world-helm \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace demo-app \
--sync-option CreateNamespace=true \
--parameter namespace=demo-app

# DELETE
argocd app delete hello-world-helm -y

