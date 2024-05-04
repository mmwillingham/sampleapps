The aws-cli folder has a deployment and better documentation.

Replace with actual value
AWS_ROLE_ARN
Namespace
Service Account



Commands to test

# Create bucket
aws s3api create-bucket --bucket mmw-bucket-us-east-1 --region us-east-1

# Uploaded file
echo "hello world" > /tmp/my-file.yaml
aws s3api put-object --bucket mmw-bucket-us-east-1 --key folder1/my-file.yaml --body /tmp/my-file.yaml

# List files
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
