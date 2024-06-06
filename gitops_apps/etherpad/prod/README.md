```

 NOTE: This does not work unless the application is named "etherpad".
 Instead, run with oc apply
 oc apply -f https://raw.githubusercontent.com/mmwillingham/sampleapps/main/gitops_apps/etherpad/prod/app-prod.yaml
 oc apply -f https://raw.githubusercontent.com/mmwillingham/sampleapps/main/gitops_apps/etherpad/stage/app-stage.yaml

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
argocd app delete etherpad-prod -y --grpc-web; oc delete project etherpad-prod; oc patch Application etherpad-prod -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge

```

