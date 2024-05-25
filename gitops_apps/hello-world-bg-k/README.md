# NOTE: Currently broken with warning about shared namespace


## Namespace/hello-world-bg is part of applications openshift-gitops/hello-world-bg-k-green and hello-world-bg-k-blue
## Route/hello-quay is part of applications openshift-gitops/hello-world-bg-k-green and hello-world-bg-k-blue


# Show layout and files
# Repo: https://github.com/mmwillingham/sampleapps/tree/main/gitops_apps/hello-world-blue-green
    Deployment - Blue
        1 replica
        Image tag: 1.0
    Deployment - Green
        2 replicas
        Image tag: latest
    Route - weight to each service

# Hello World - blue green - BLUE
argocd app create hello-world-bg-k-blue \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world-bg-k/overlays/blue \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace hello-world-bg

argocd app delete hello-world-bg-k-blue -y

# Hello World - blue green - GREEN
argocd app create hello-world-bg-k-green \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world-bg-k/overlays/green \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--dest-namespace hello-world-bg

argocd app delete hello-world-bg-k-green -y

# Weighted 100% to blue
https://hello-quay-hello-world-bg.apps.bosez-20240521.5nay.p1.openshiftapps.com/
Image version : v1.0

# Weighted 100% to green
https://hello-quay-hello-world-bg.apps.bosez-20240521.5nay.p1.openshiftapps.com/
Image version : latest
