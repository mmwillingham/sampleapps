argocd app create toolbox \
--repo https://github.com/mmwillingham/toolbox-container.git \
--path deploy \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--sync-option CreateNamespace=true \
--dest-namespace toolbox

# DELETE
argocd app delete toolbox -y