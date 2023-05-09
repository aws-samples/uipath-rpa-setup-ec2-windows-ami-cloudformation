# UiPath RPA Bot Setup using CloudFormation

## Target Architecture

![Architecture](images/Architecture.jpeg)

## Deployment Steps

**Download UiPath file and upload in S3 bucket**
- Download UiPath msi file from the [location](https://download.uipath.com/UiPathStudioCommunity.msi)
- Upload the UiPath msi file into Amazon S3 bucket as per the [documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html)
- Update the bucket name and file key in cloudformation template ec2-image-builder.yaml in the user data [line number 310](https://github.com/aws-samples/uipath-rpa-setup-ec2-windows-ami-cloudformation/blob/main/ec2-image-builder.yml#L310).

**Deploy EC2 Image Builder Pipeline**
- Clone / download, EC2 image pipeline CloudFormation template from this [repository](https://github.com/aws-samples/uipath-rpa-setup-ec2-windows-ami-cloudformation/blob/main/ec2-image-builder.yml)
- Login to [AWS Console](https://aws.amazon.com/console/)
- Navigate to [CloudFormation console](https://aws.amazon.com/console/).
- Create stack as per this [documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) .
- Monitor stack events

**Deploy CloudFormation Macros**
- Clone or download the [repository](https://github.com/aws-cloudformation/aws-cloudformation-macros).
- Navigate to Count folder.
- You will need a S3 bucket to store the CloudFormation artifacts:
- If you don't have one already, create one with `aws s3 mb s3://<bucket name>`
- Package the Macro CloudFormation template. The provided template uses the AWS Serverless Application Model so must be transformed before you can deploy it.

```
    aws cloudformation package \
    --template-file template.yaml \
    --s3-bucket <your bucket name here> \
    --output-template-file packaged.yaml
```

For example: 
    `aws cloudformation package --template-file template.yaml --s3-bucket count-macro-ec2 --output-template-file packaged.yaml`
 Deploy the packaged CloudFormation template to a CloudFormation stack:

```
    aws cloudformation deploy \
    --stack-name Count-macro \
    --template-file packaged.yaml \
    --capabilities CAPABILITY_IAM
```

To test out the macro's capabilities, try launching the provided example template:

```
    aws cloudformation deploy \
    --stack-name Count-test \
    --template-file test.yaml \
    --capabilities CAPABILITY_IAM
```
**Deploy EC2 Provisioning CloudFormation**

- Clone / download, EC2 Provisioning CloudFormation template (ec2-provisioning.yaml) from this [repository](https://github.com/aws-samples/uipath-rpa-setup-ec2-windows-ami-cloudformation/blob/main/ec2-provisioning.yaml)
- Login to AWS Console
- Navigate to CloudFormation console.
- Create stack as per this [documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)
- Monitor stack events.

