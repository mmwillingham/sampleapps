argocd app create hello-quay-bg \
--repo https://mmwillingham.github.io/sampleapps \
--helm-chart hello-quay \
--revision 0.0.1 \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--sync-option CreateNamespace=true \
--project default \
--dest-namespace hello-quay-bg

# Delete
argocd app delete hello-quay-bg -y

argocd app delete hello-quay-bg -y --grpc-web; oc delete project hello-quay-bg; oc patch Application hello-quay-bg -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge