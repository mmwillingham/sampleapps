This deployment allow an AWS cli to connect to AWS resources using a service account.
Replace AWS_ROLE_ARN with actual value


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
https://github.com/mmwillingham/sampleapps/blob/main/aws-cli/aws-cli-deploy.yaml

## Verify pods have STS info
oc get pod -l app=awscli-sts-debug -ojson | jq '.spec.containers[].env'
oc get pod -l app=awscli-sts-debug -ojson | jq '.spec.containers[].volumeMounts[]'





##Commands to test
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
