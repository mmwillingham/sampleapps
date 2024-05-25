# Quick demo of argocd / gitops

argocd app create hello-world \
--repo https://github.com/mmwillingham/sampleapps.git \
--path gitops_apps/hello-world \
--dest-server https://kubernetes.default.svc \
--sync-policy automated \
--self-heal \
--sync-option Prune=true \
--sync-option CreateNamespace=true \
--dest-namespace hello-world

# Delete
argocd app delete hello-world -y

# Show in action
oc project hello-world
watch oc get pods

# Second terminal - split screen
oc scale deployment hello-world --replicas=4
# watch scale up and back down

oc get route 
oc delete route
oc get route

# Change replicas in github and sync

# Canary (rolling update)
## Show strategy - deployment in argoCD
## Change version to v1.0   Show that always 3 versions are available
        #image: quay.io/redhattraining/do480-hello-app:v1.0
        image: quay.io/redhattraining/do480-hello-app:latest

# First terminal:
watch oc get pods

# second terminal:
while true; do curl https://hello-quay-hello-world.apps.bosez-20240521.5nay.p1.openshiftapps.com/; sleep 1; done
