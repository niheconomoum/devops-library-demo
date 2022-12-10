# devops-library-demo

[InCommon Advance CAMP (ACAMP) 2022—Build Your Own Docker Official Images](https://docs.google.com/document/d/1zqekiGB7FDq4tOrmAfZWh0G_eKBNHnDaebP1mJhO2Fc/edit)

## Installation

Copy this repository to a supported hosting service (e.g., GitHub) and deploy the CI/CD pipeline using CloudFormation.

```bash
git clone git@github.com/example/devops-library-demo
cd devops-library-demo
ECR_PUBLIC_ALIAS=$(
    aws ecr-public describe-registries \
        --query 'registries[0].aliases[0].name' \
        --output text
)
aws cloudformation create-stack --stack-name devops-library-demo \
    --template-body file://.cloudformation/pipeline.yaml \
    --capabilities CAPABILITY_IAM \
    --parameters \
        ParameterKey=GitRepoId,ParameterValue=example/devops-library-demo \
        ParameterKey=RegistryServer,ParameterValue=public.ecr.aws \
        ParameterKey=RegistryOwner,ParameterValue=${ECR_PUBLIC_ALIAS} \
    ;
```

In the AWS console, navigate to the [AWS Developer Tools settings](https://console.aws.amazon.com/codesuite/settings/connections). Select the connection with the same name as the CloudFormation stack, e.g., **devops-library-demo**, and click **Update pending connection**. Choose an existing app or install a new one if necessary. When installing a new app, grant the AWS Connector for GitHub access to at least that specific repository in your organization. For further instructions on how to complete the connection, refer to [Create a connection to GitHub](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-create-github.html) and [Update a pending connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-update.html) in the [AWS Developer Tools console](https://docs.aws.amazon.com/dtconsole/latest/userguide/welcome-connections.html) user guide.

After making the connection to GitHub, navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home). Select the CloudFormation stack, e.g., **devops-library-demo**. On the stack outputs tab, click the value of PipelineUrl to open the CI/CD pipeline. On the pipeline detail page, click **Release change**, which builds and publishes Docker images from the configured GitHub repository (this repository) for the first time.

## Coding Style

Please follow [the Python Style Guide (PEP8)](https://pep8.org/), [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), [AWS CloudFormation best practices](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html), and [the Home Assistant YAML style guide](https://developers.home-assistant.io/docs/documenting/yaml-style-guide/) as appropriate.

In CloudFormation templates, please follow [the recommended template section ordering](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html#template-anatomy-sections).

In shell scripts, indent using single tabs instead of spaces.

## Git Commit Messages

Please follow [Angular Commit Message Conventions](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format). The following scopes are currently in use:
- **pipeline**: the CloudFormation stack template defining the CI/CD pipeline
- **buildspec**: the CodeBuild specification
- **license**: software licensing information, specifically [LICENSE.md](LICENSE.md)
- **readme**: this file
