```

argocd app create etherpad-umbrella-blue \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/etherpad-umbrella/blue \
--dest-server https://kubernetes.default.svc \
--sync-policy automated  \
--self-heal \
--sync-option Prune=true \
--dest-namespace etherpad-umbrella-blue  \
--sync-option CreateNamespace=true \
--grpc-web



# DELETE
argocd app delete etherpad-umbrella-blue -y
argocd app delete etherpad-umbrella-blue -y --grpc-web; oc delete project etherpad-umbrella-blue ; oc patch Application etherpad-umbrella-blue -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge

```

