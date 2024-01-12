## https://medium.com/@vishwasacharya/how-to-set-up-a-ci-cd-pipeline-in-azure-devops-a-step-by-step-guide-9a6633422aa2

## Set up permission to create branches 
https://learn.microsoft.com/en-us/azure/devops/repos/git/set-git-repository-permissions?view=azure-devops#git-repository

## [Azure DevOps Product Development Multiple Teams](https://www.dotnetcurry.com/devops/azure-product-development-multiple-teams)

## [Azure devops pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)

## [adf: Automated publishing for continuous integration and delivery](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery-improvements)

## DevOps extersion install  
- Databricks Script Deployment 
- Terreform 
- Submit Azure DevOps Parallelism Request Form if using MSFT hosting Agent    

## Pipeline Library : Variable Group + Variable  

## Devops: Create a service connection:  
#### Create a service principle :     
- Azure Active Directory / App Registrations / +New Registration / Name : sp-devops-pipeline:    
- service-Principle Page: Certificates & Secret    / +New Client Secret 
- Your Subscription: / Access Control (IAM) /Add New Role Assignment / select Contributor role --> Next/ Select Member --> type sp-devops-pipeline as member or service principle
## Create DevOps Service connection: 
- DevOps: Organization/Setting/Service Connection /Service Priciple (Manual)  
- fill in the Subscription ID, Subscription Name, Service Priciple ID and Service Priciple Key + Tenant ID --> click to verify.
- fill in the service connection name and description   
- check 'Grant Access Permission to All Pipeline'
- Verify and save  

## YouTube: 

- [Continuous Integration, Continuous Deployment CI-CD with Azure DevOps](https://www.youtube.com/watch?v=jRgLSMlp28U&t=26s)

- [DevOps Pipeline, ADF, Databricks... by Riz Ang](https://www.youtube.com/watch?v=DTQD3Fqfxbc&list=PL8mxdtDlQDd8ve2ZhdEIVAaeb1HpV47Af&index=2)

## Reference:  
- https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops


