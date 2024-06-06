argocd app create hello-quay-bg \
--repo https://mmwillingham.github.io/sampleapps \
--helm-chart hello-quay-bg \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--sync-option CreateNamespace=true \
--project default \
--dest-namespace hello-quay-bg \
--revision 0.0.6


while true; do curl https://hello-quay-hello-quay-bg.apps.bosez-20240521.5nay.p1.openshiftapps.com/; sleep 1; done



# Delete
argocd app delete hello-quay-bg -y

argocd app delete hello-quay-bg -y --grpc-web; oc delete project hello-quay-bg; oc patch Application hello-quay-bg -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge