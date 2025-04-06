pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        RESOURCE_GROUP = 'rg-jenkins'
        APP_SERVICE_NAME = 'reacttjenkinskeshav'
        TERRAFORM_PATH = '"C:\\Users\\user\\Downloads\\terraform_1.11.3_windows_386\\terraform.exe"'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/keshav99281/react_jenkins.git'
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Debug Workspace') {
            steps {
                bat 'dir'
            }
      }
      stage('Terraform Init') {
           steps {
                bat '"%TERRAFORM_PATH%" -chdir=terraform init '
          }
    }
      stage('Terraform Plan & Apply') {
           steps {
               
               bat '"%TERRAFORM_PATH%" -chdir=terraform plan -out=tfplan'
               bat '"%TERRAFORM_PATH%" -chdir=terraform apply -auto-approve tfplan'
           }
     }

        stage('Debug my-app Directory') {
          steps {
            dir('my-app') {
               bat 'dir'
           }
         }
       }

        
        stage('Build') {
            steps {
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat "az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID"
                    bat "powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force"
                    bat "az webapp deploy --resource-group $RESOURCE_GROUP --name $APP_SERVICE_NAME --src-path ./publish.zip --type zip"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
