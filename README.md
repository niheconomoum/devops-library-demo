# devops-library

## Installation

Clone this repository locally and deploy the CI/CD pipeline in the us-east-1 region using CloudFormation:

```bash
git clone git@github.com/ResearchDataCom/devops-library
cd devops-library
aws cloudformation create-stack --stack-name rdctdev-devops-library-us-east-1 \
    --template-body file://.cloudformation/main.yaml \
    --capabilities CAPABILITY_IAM \
    --parameters \
        ParameterKey=GitRepoId,ParameterValue=ResearchDataCom/devops-library \
        ParameterKey=RegistryServer,ParameterValue=public.ecr.aws \
        ParameterKey=RegistryOwner,ParameterValue=c4c0z6q5 \
    --region us-east-1 \
    ;
```

In the AWS console, navigate to the [AWS Developer Tools settings (us-east-1)](https://us-east-1.console.aws.amazon.com/codesuite/settings/connections?region=us-east-1).
Select the connection with the same name as the CloudFormation stack, e.g., **rdctdev-devops-library-us-east-1**, and click **Update pending connection**.
Choose an existing app or install a new one if necessary.
When installing a new app, grant the AWS Connector for GitHub access to all repositories in the ResearchDataCom organization.
For further instructions on how to complete the connection, refer to [Create a connection to GitHub](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-create-github.html) and [Update a pending connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-update.html) in the [AWS Developer Tools console](https://docs.aws.amazon.com/dtconsole/latest/userguide/welcome-connections.html) user guide.

After making the connection to GitHub, navigate to the [AWS CloudFormation console (us-east-1)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1).
Select the CloudFormation stack, e.g., **rdctdev-devops-library-us-east-1**.
On the stack outputs tab, click the value of PipelineUrl to open the CI/CD pipeline.
On the pipeline detail page, click **Release change**, which builds and publishes Docker images from the configured GitHub repository (this repository) for the first time.

## Contributing

### Coding Style

The following Git commit scopes are currently in use:
- **cfn**: anything else related to the CloudFormation template that deploys the CI/CD pipline; includes [.cloudformation/main.yaml](.cloudformation/main.yaml)
- **buildspec**: the project build and deploy scripts; includes [.awscodepipeline/buildspec.yaml](.awscodepipeline/buildspec.yaml)
- **readme**: this file
