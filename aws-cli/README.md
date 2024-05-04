This deployment allow an AWS cli to connect to AWS resources using a service account.
AWS prereqs:
- Create IAM trust policy
- Create IAM role
- Attach them together

This working doc has the steps:
https://docs.google.com/document/d/1nGuHfKrjHxxfEMnDsQS2gNqXOvV43SvZ7m66D87qTDw/edit

Important info:
    - In this yaml, replace with actual values
    -- AWS_ROLE_ARN
    -- Namespace
    -- Service Account


## Create namespace
oc new-project test-ns

## Create service account
### This is already included in the yaml
oc create sa test-sa

## Annotate service account -- new environment variables and mounts will be added to the pods
### This is already included in the yaml. To do it manually:
oc annotate sa test-sa eks.amazonaws.com/role-arn=${ROLE_ARN}

## Set service account in the deployment
### This is already included in the yaml. To do it manually:
oc set sa deployment awscli-sts-debug test-sa

## Verify service account is annotated
oc describe sa test-sa
### Output (only showing annotations)
Annotations:         eks.amazonaws.com/role-arn: arn:aws:iam::519926745982:role/bosez-gdabs-Rosa-s3

## Create deployment
https://github.com/mmwillingham/sampleapps/blob/main/aws-cli/awscli-sts-debug.yaml

## Verify pods have STS info
oc get $(oc get pod -l app=awscli-sts-debug -oname) -ojson | jq -r '.spec.containers[].env'
    ### There should be a correct value for AWS_ROLE_ARN and AWS_WEB_IDENTITY_TOKEN_FILE
    AWS_WEB_IDENTITY_TOKEN_FILE might look like one of these - both seem to work
    "value": "/var/run/secrets/eks.amazonaws.com/serviceaccount/token"
    "value": "/var/run/secrets/openshift/serviceaccount/token"


oc get $(oc get pod -l app=awscli-sts-debug -oname) -ojson | jq -r '.spec.containers[].volumeMounts'
    ### There should be a key named aws-iam-token

oc get $(oc get pod -l app=awscli-sts-debug -oname) -ojson | jq -r '.spec.volumes[].name'
    ### There should be a key named aws-iam-token



## Commands to test
## Open a terminal session in the pod
oc debug deploy/awscli-sts-debug

## Create bucket
aws s3api create-bucket --bucket mmw-bucket-us-east-1 --region us-east-1

## Uploaded file
echo "hello world" > /tmp/my-file.yaml
aws s3api put-object --bucket mmw-bucket-us-east-1 --key folder1/my-file.yaml --body /tmp/my-file.yaml

## List files
aws s3api list-objects --bucket mmw-bucket-us-east-1
{
    "Contents": [
        {
            "Key": "folder1/my-file.yaml",
            "LastModified": "2024-05-04T13:51:00+00:00",
            "ETag": "\"6f5902ac237024bdd0c176cb93063dc4\"",
            "Size": 12,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "sandbox2129",
                "ID": "9b7b0b277f37cc4aaf74270f4774043841e36faccef8e5d41d718f3b5ffd0d4d"
            }
        }
    ],
}


sh-4.2# aws s3 ls mmw-bucket-us-east-1 
                           PRE folder1/
