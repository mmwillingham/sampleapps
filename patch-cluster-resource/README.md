Use argocd (GitOps) to update an existing cluster resource that is not controlled by argocd.
https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#server-side-apply

# This does not work as expected. It throws error that replicas is required.

## Steps
### Create a cluster resource manually / not with argocd
oc new-project argocd-patch

cat << EOF > deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-quay
  labels:
    app: hello-quay
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-quay
  template:
    metadata:
      labels:
        app: hello-quay
    spec:
      containers:
      - name: hello-quay
        image: quay.io/redhattraining/do480-hello-app:v1.0
EOF

oc apply -f deploy.yaml -n argocd-patch

### Place deployment.yaml with these contents in your git repo
apiVersion: apps/v1
kind: Deployment
metadata:
 labels:
   foo: bar                    # new label
name: hello-quay
namespace: argocd-patch

### Label the namespace to give argocd access
oc label ns argocd-patch argocd.argoproj.io/managed-by=openshift-gitops

### Verify label does not exist
oc get deploy hello-quay -n argocd-patch --show-labels

### Create an argocd app with ServerSideApply=true
argocd app create argocd-patch --repo https://github.com/mmwillingham/sampleapps.git --path patch-cluster-resource --dest-server https://kubernetes.default.svc --sync-policy automated --sync-option ServerSideApply=true --sync-option Validate=false --dest-namespace argocd-patch

### Verify label does exist
oc get deploy hello-quay -n argocd-patch --show-labels

