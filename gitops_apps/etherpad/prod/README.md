```
To create the helm repo
cat << EOF > add_repo.yaml
apiVersion: helm.openshift.io/v1beta1
kind: HelmChartRepository
metadata:  
  name: mmwillingham-repo  
spec:
  connectionConfig:
    url: 'https://mmwillingham.github.io/sampleapps/'
  name: MMW Helm Charts
EOF
oc apply -f add_repo.yaml

NOTE: This does not work unless the application is named "etherpad".
Instead, run with oc apply
oc apply -f https://raw.githubusercontent.com/mmwillingham/sampleapps/main/gitops_apps/etherpad/prod/app-prod.yaml

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

