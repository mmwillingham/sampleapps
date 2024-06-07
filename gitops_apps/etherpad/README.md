```
# This will create stage and prod:
oc apply -k https://github.com/mmwillingham/sampleapps/gitops_apps/helm-master-app/base


# DELETE
argocd app delete etherpad-prod -y
argocd app delete etherpad-prod -y --grpc-web; oc delete project etherpad-prod; oc patch Application etherpad-prod -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge

argocd app delete etherpad-stage -y
argocd app delete etherpad-stage -y --grpc-web; oc delete project etherpad-stage; oc patch Application etherpad-stage -n openshift-gitops -p '{"metadata":{"finalizers":null}}' --type=merge






# To create the helm repo
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

```

