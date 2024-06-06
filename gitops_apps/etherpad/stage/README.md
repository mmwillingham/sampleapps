```
NOTE: This does not work unless the application is named "etherpad".
Instead, run with oc apply
oc apply -f https://raw.githubusercontent.com/mmwillingham/sampleapps/main/gitops_apps/etherpad/stage/app-stage.yaml

argocd app create etherpad-stage \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/etherpad/stage \
--dest-server https://kubernetes.default.svc \
--sync-policy automated  \
--self-heal \
--sync-option Prune=true \
--dest-namespace etherpad-stage \
--sync-option CreateNamespace=true \
--grpc-web


# DELETE
argocd app delete etherpad-stage -y
argocd app delete etherpad-stage -y --grpc-web; oc delete project etherpad-stage; oc patch Application etherpad-stage -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge

```