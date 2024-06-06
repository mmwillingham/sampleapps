```
oc apply -f https://raw.githubusercontent.com/mmwillingham/sampleapps/main/gitops_apps/etherpad/stage/app-stage.yaml

# DELETE
argocd app delete etherpad-stage -y
argocd app delete etherpad-stage -y --grpc-web; oc delete project etherpad-stage; oc patch Application etherpad-stage -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge

```