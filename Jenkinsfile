pipeline {
    agent any
    tools {
        terraform 'terraform'
    }
    stages {
    
    stage('Terraform Init') {
      steps {
        sh "cd terraform-azure-aks && terraform init -input=false"
      }
    }
    
    stage('Terraform Plan') {
            steps {
               withCredentials([azureServicePrincipal(
                credentialsId: '72638069-0faa-468f-8b96-bcc88be5343f',
                subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                clientIdVariable: 'ARM_CLIENT_ID',
                clientSecretVariable: 'ARM_CLIENT_SECRET',
                tenantIdVariable: 'ARM_TENANT_ID')]) {
                        sh "cd terraform-azure-aks && terraform plan -out=tfplan -input=false -var-file='terraform.tfvars'"
                        }
                }
        }
    

    stage('Terraform Apply & Destroy') {
            steps {
               withCredentials([azureServicePrincipal(
                credentialsId: '72638069-0faa-468f-8b96-bcc88be5343f',
                subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                clientIdVariable: 'ARM_CLIENT_ID',
                clientSecretVariable: 'ARM_CLIENT_SECRET',
                tenantIdVariable: 'ARM_TENANT_ID')]) {
                        sh "cd terraform-azure-aks && terraform apply -input=false --auto-approve tfplan "
                        }
                }
        }
   }
}
