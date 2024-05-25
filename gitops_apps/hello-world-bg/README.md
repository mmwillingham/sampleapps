
argocd app create hello-world-bg \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world-bg/base \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace hello-world-bg

argocd app delete hello-world-bg -y