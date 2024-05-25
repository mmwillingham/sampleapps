# Install GitOps operator
cat << EOF > gitops_operator.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: openshift-gitops-operator
---
apiVersion: v1
kind: Namespace
metadata:
  name: openshift-gitops
  labels:
    openshift.io/cluster-monitoring: 'true'
---
apiVersion: operators.coreos.com/v1alpha2
kind: OperatorGroup
metadata:
  name: openshift-gitops-operator
  namespace: openshift-gitops-operator
spec:
  upgradeStrategy: Default
---
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: openshift-gitops-operator
  namespace: openshift-gitops-operator
spec:
  channel: latest
  installPlanApproval: Automatic
  name: openshift-gitops-operator
  source: redhat-operators
  sourceNamespace: openshift-marketplace
#   config:
#     env:
#       - name: DISABLE_DEFAULT_ARGOCD_INSTANCE
#         value: 'true'
#       - name: ARGOCD_CLUSTER_CONFIG_NAMESPACES
#         value: 'openshift-gitops'
---
EOF

oc apply -f gitops_operator.yaml

# Give gitops control over all namespaces
# Alternate is to label desired namespace: "oc label ns <ns-name> argocd.argoproj.io/managed-by=openshift-gitops"
cat << EOF > argocd-rbac-ca.yaml
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: argocd-rbac-ca
subjects:
  - kind: ServiceAccount
    name: openshift-gitops-argocd-application-controller
    namespace: openshift-gitops
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
EOF
oc apply -f argocd-rbac-ca.yaml
...

# Retrieve and use argocd admin_password to login:
ADMIN_PASSWD=$(oc get secret openshift-gitops-cluster -n openshift-gitops -o jsonpath='{.data.admin\.password}' | base64 -d)
SERVER_URL=$(oc get routes openshift-gitops-server -n openshift-gitops -o jsonpath='{.status.ingress[0].host}')
argocd login --username admin --password ${ADMIN_PASSWD} ${SERVER_URL} --insecure
argocd app list

## Add step to assign certificate
