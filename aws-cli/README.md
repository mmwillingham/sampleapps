This deployment allow an AWS cli to connect to AWS resources using a service account.


# Create namespace
oc new-project test-ns

# Create service account
# This is already included in the yaml
oc create sa test-sa

# Annotate service account -- new environment variables and mounts will be added to the pods
# This is already included in the yaml. To do it manually:
oc annotate sa test-sa eks.amazonaws.com/role-arn=${ROLE_ARN}

# Set service account in the deployment
# This is already included in the yaml. To do it manually:
oc set sa deployment awscli-sts-debug test-sa

# Verify
oc describe sa test-sa
# Output (only showing annotations)
Annotations:         eks.amazonaws.com/role-arn: arn:aws:iam::519926745982:role/bosez-gdabs-Rosa-s3

# Create deployment
# https://github.com/mmwillingham/sampleapps/blob/main/aws-cli/aws-cli-deploy.yaml

