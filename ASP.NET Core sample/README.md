# Sample ASP.NET Core app

This sample showcases an yaml based Azure DevOps CI / CD Pipeline that does the following:
* Uses a multistage docker image to compile the ASP.NET Core app
* Builds a final image with only the required run time and the compiled code
* Pushes the code to our Shared ECR Prod image repository
* Replaces variables in Kubernetes manifest files
* Deploys the application to Kubernetes

## Requirements

This sample relies on two service connections in your Azure DevOps project and an existing ECR repository to push your image to.

### Service Connections

#### Kubernetes Service Connection

If you don't already have a kubernetes service connection that uses the SAML based kubernetes config file, you will need to create it inside your Azure DevOps project.

1. Go to the parameter store of your Capability's AWS account, make sure you're in *eu-central-1*
2. Copy the value of parameter '/managed/deploy/kube-config', and use it in the instructions of the following step
3. Follow the [instructions to setup an Azure DevOps Service Connection][PB-ADOsvc].
4. Use the file you downloaded instead of the one described in the *Obtaining config* files steps in the article.
5. Name the Service Connection something that makes it easy to recognize like: `Kubernetes-Hellman-{CAPABILITY_ROOT_ID}`

#### AWS ECR Service Connection

If you don't already have a Service Connection in your Azure DevOps project for AWS ECR Push Prod or similar that allows you to push to the shared ECR repository, you need to request this for your project in TopDesk or in the #dev-peer-support slack channel.

If you do not have an image repository in the shared ECR you will need to request this as well.

<!----------------- Links ----------------->
[PB-info]: https://wiki.dfds.cloud/en/playbooks/deployment/obtain-info
[PB-auth]: https://wiki.dfds.cloud/en/playbooks/deployment/authentication
[PB-ADOsvc]: https://wiki.dfds.cloud/en/playbooks/deployment/k8s-service-connection
