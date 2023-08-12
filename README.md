This project explains the deployment of a .NET application to azure kubernetes service AKS using docker terraform and YAML pipeline.

Workflow:

Create the .NET application (Julapp) using Visual studio.

Write a docker file to package the application in the form of a container image. 
The dockerfile should be placed in the root folder of the application.

Write the terraform confgiuration file to provision AKS and azure container registry ACR.

The terraform code main.tf deploys the resource AKS and ACR to azure and also grants AKS the needed roles to pull images from ACR.

The service principal / Managed identity / user authenticating to azure from the terminal needs to have the contributor role on the subscription level for the role assignemet to be adequately provisioned.

Write the YAML pipeline to deploy the push based CI/CD workflow.

The YAML pipeline (azure-pipelines.yml) consists of 4 stages

Terraform Validate:
This installs terraform and initialises it with our remote backend before checking if our code is valid

Terraform Deploy:
This refreshes the state file before deploying main.tf to azure

Docker_Build_and_Push:
This builds our application in the form of a container image before pushing it to our private registry ACR which has already been provisioned on azure
Also the manifest file julimage-deployment.yaml is uploaded as an artifact to our current operation.

Continuous Deployment stage

Deploy_to_AKS:
Downloads the artifact which will be used alongside the image on ACR to create the deployment and service.
An environment jul-aks was incoporated into the pipeline to grant real time access to the Kubernetes cluster from Azure Devops


