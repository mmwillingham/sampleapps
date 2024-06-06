argocd app create hello-quay \
--repo https://mmwillingham.github.io/sampleapps \
--helm-chart hello-quay \
--revision 0.0.1 \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--sync-option CreateNamespace=true \
--project default \
--dest-namespace hello-quay

# Delete
argocd app delete hello-quay -y

argocd app delete hello-quay -y --grpc-web; oc delete project hello-quay; oc patch Application hello-quay -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge