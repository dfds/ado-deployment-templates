# Sample ASP.NET Core app

This sample showcases an yaml based Azure DevOps CI / CD Pipeline that does the following:
* Uses a multistage docker image to compile the ASP.NET Core app
* Builds a final image with only the required run time and the compiled code
* Pushes the code to our Shared ECR Prod image repository
* Replaces variables in Kubernetes manifest files
* Uses an Active Directory service and SAML authentication for Kubernetes
* Deploys the application to Kubernetes

## Requirements

This sample relies on two service connections in your Azure DevOps project and an existing ECR repository to push your image to.
You will also need to add some Azure DevOps pipeline variables with credentials for SAML2AWS in order to authenticate with Kubernetes.

### Service Connections

#### Kubernetes Service Connection

If you don't already have a kubernetes service connection that uses the SAML based kubernetes config file, you will need to create it inside your Azure DevOps project.

1. Download our [SAML based Kubernetes config][K8S-config] file.
2. Follow the [instructions to setup an Azure DevOps Service Connection][PB-ADOsvc].
3. Use the file you downloaded instead of the one described in the *Obtaining config* files steps in the article.
4. Name the Service Connection something that makes it easy to recognize like: `Kubernetes-Hellman-Saml2aws`

This service connection can be reused for all deployments in your Azure DevOps project for pipelines that uses SAML2AWS for authentication, regardless of the namespace you deploy to as this information is supplied on a pipeline to pipeline basis via variables.

#### AWS ECR Service Connection

If you don't already have a Service Connection in your Azure DevOps project for AWS ECR Push Prod or similar that allows you to push to the shared ECR repository, you need to request this for your project in the #dev-excellence slack channel.

If you do not have an image repository in the shared ECR you will need to request this as well.

### Authentication

To successfully run this pipeline, some session variables must be defined as pipeline variables in order to get your SAML2AWS authentication to work in an unattended pipeline.  

Follow the instructions in the [Obtaining info needed for pipeline][PB-info] playbook article to get the various values.  

When you have obtained the values, add the following variables to your Azure DevOps pipeline:  
* AWS_PROFILE
* SAML2AWS_PASSWORD
* SAML2AWS_ROLE
* SAML2AWS_USERNAME

Fill in the variables with the values as described in the [Authenticating against AWS and Kubernetes][PB-auth] playbook.  
:exclamation: It is important to use capital letters for the variable names as this will mount them as Environment Variables.

<!----------------- Links ----------------->
[PB-info]: https://wiki.dfds.cloud/en/playbooks/deployment/obtain-info
[PB-auth]: https://wiki.dfds.cloud/en/playbooks/deployment/authentication
[K8S-config]: https://dfds-oxygen-k8s-public.s3-eu-west-1.amazonaws.com/kubeconfig/hellman-saml.config
[PB-ADOsvc]: https://wiki.dfds.cloud/en/playbooks/deployment/k8s-service-connection