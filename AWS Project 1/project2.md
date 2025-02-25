## Documentation for Amazon ECS

Create IAM user
1. grant administration full access
2. grant ec2 full access
3. https://git-codecommit.us-east-1.amazonaws.com/v1/repos/aws-ecs-docker

download cred from IAM user from(HTTPS Git credentials for AWS CodeCommit )

4. create a repo on ECR
you need to edit the account ID and region in the file(buildspec.yml)


5. to create a build project
go to the code build and create
 group name: aws-ecs-docker-logs
 attach role(codebuild-aws-ecs-docker-service-role) to codebuild-aws-ecs-docker-service-role
 a. aws iam attach-role-policy --role-name codebuild-aws-ecs-docker-service-role --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
b. aws iam attach-role-policy --role-name codebuild-aws-ecs-docker-service-role --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess

# Next Building ECS cluster
to set up the core infrastructure we use CloudFormation to set up and deploy infrastructure

1. open create formation
2. create new stack and upload core-infrasture.setup.yml file(https://github.com/bwrutu/aws_snippets/tree/main/ecs-cli/core-infrastructure)
3. setup ecs cluster with ec2 as the deployment engine upload template(create a role and attach AmazonEC2ContainerServiceforEC2Role policy to the role and name the role "ecsInstanceRole")-------aws cloudformation delete-stack --stack-name my-stack------
4. setup role CodeBuildRole with policies(codeBuildBatchPolicy, codeBuildServiceRolePolicy & AmazonEC2ContainerRegistryFullAccess) on IAM user
5. Create a task definition(A task definition is a blueprint that describes how a docker container should be launched)
a. Go to ECS > choose task definition
b. to create task role> 
  create a policy using the ecsTaskrole.json file, then create a role(ECS as service or use case) and attach ecsTaskrole.json to it
c.create service(A service defines a long-running tasks of the same task definition, which can be one-running container or multiple-running containers using the same task definition.) ##pipeline

6. Now build a AWS pipeline using AWS CodePipeline
