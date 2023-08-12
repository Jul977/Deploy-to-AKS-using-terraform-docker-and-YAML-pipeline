**This project explains the deployment of a .NET application to Azure Kubernetes service AKS using docker terraform and YAML pipeline.**

**Workflow:**

Create the .NET application (Julapp) using Visual Studio.

Write a docker file to package the application in the form of a container image. 
The docker file should be placed in the root folder of the application.

Write the terraform confgiuration file to provision AKS and azure container registry ACR.

The terraform code main.tf deploys the resource AKS and ACR to Azure and also grants AKS the needed roles to pull images from ACR.

The service principal / Managed identity/user authenticating to Azure from the terminal needs to have the contributor role on the subscription level for the role assignment to be adequately provisioned.

Write the YAML pipeline to deploy the push-based CI/CD workflow.

The YAML pipeline (azure-pipelines.yml) consists of 4 stages.

![cicd](https://github.com/Jul977/Deploy-to-AKS-using-terraform-docker-and-YAML-pipeline/assets/110497123/d9b8ac67-6437-44f6-84b0-d3de0ac965de)

Terraform Validate:
This installs terraform and initialize it with our remote backend before checking if our code is valid

Terraform Deploy:
This refreshes the state file before deploying main.tf to Azure

Docker_Build_and_Push:
This builds our application in the form of a container image before pushing it to our private registry ACR which has already been provisioned on Azure
Also, the manifest file julimage-deployment.yaml is uploaded as an artifact to our current operation.

Continuous Deployment stage

Deploy_to_AKS:
Downloads the artifact which will be used alongside the image on ACR to create the deployment and service.
An environment jul-aks was incorporated into the pipeline to grant real-time access to the Kubernetes cluster from Azure DevOps.

![k8prod](https://github.com/Jul977/Deploy-to-AKS-using-terraform-docker-and-YAML-pipeline/assets/110497123/3e56fbae-3350-4733-b100-987e0ce03a3e)


Finally, the application can be accessed on the browser using the public Ip of the load balancer on port 80 which was defined in the manifest file.



