pipeline {
    agent any
    tools {
        terraform 'terraform'
    }
    stages {
    
    stage('Terraform Init') {
      steps {
        sh "cd terraform-aks-k8s && terraform init -backend-config="storage_account_name=terraformaksstrgact" -backend-config="container_name=tfstate" -backend-config="access_key=B0MzLNte4wL+A7DRgUYk4uKEjUYFFLq2G2XG3UdnN912ZwFJihlDM6EXolJXEp6/t4opkn6zZVYY+AStsH/pIg==" -backend-config="key=codelab.microsoft.tfstate"
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
                        sh "cd terraform-aks-k8s && terraform plan -out=tfplan "
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
                        sh "cd terraform-aks-k8s && terraform apply out.plan --auto-approve "
                        }
                }
        }
   }
}
