# To create a helm repo
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