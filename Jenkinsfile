pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        AZURE_RG = 'rg-jenkins-react'
        AZURE_WEBAPP_NAME = 'reactjenkinskeshav'
        TERRAFORM_PATH = '"C:\\Users\\user\\Downloads\\terraform_1.11.3_windows_386\\terraform.exe"'
        REACT_APP_DIR = 'my-app'
    }

      stages {
//         stage('Clean Workspace') {
//     steps {
//         cleanWs()
//     }
// }
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/keshav99281/react_jenkins.git'
            }
        }
        
    //   stage('Terraform Init') {
    //        steps {
    //             bat '"%TERRAFORM_PATH%" -chdir=terraform init '
    //       }
    // }
    //   stage('Terraform Plan & Apply') {
    //        steps {
               
    //            bat '"%TERRAFORM_PATH%" -chdir=terraform plan -out=tfplan'
    //            bat '"%TERRAFORM_PATH%" -chdir=terraform apply -auto-approve tfplan'
    //        }
    //  }

       //  stage('Debug my-app Directory') {
       //    steps {
       //      dir('my-app') {
       //         bat 'dir'
       //     }
       //   }
       // }

        stage('Install Dependencies') {
      steps {
        dir("${REACT_APP_DIR}") {
          bat 'npm install'
        }
      }
    }


    stage('Build React App') {
      steps {
        dir("${REACT_APP_DIR}") {
          bat 'npm run build'
        }
      }
    }

        // stage('Build') {
        //     steps {
        //         bat 'npm run build'
        //     }
        // }

//         stage('File zipping') {
//     steps {
//         dir('my-app') {
            
//             bat 'mkdir ..\\publish'
            
//             bat 'xcopy build ..\\publish /E /I'
//         }
//         bat 'powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force'
//     }
// }
//         // stage('Login') {
        //     steps {
                
        //            withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
        //                bat "az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID"
        //                //bat "powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force"
        //                //bat "az webapp deployment source config-zip --resource-group $RESOURCE_GROUP --name $APP_SERVICE_NAME --src-path ./publish.zip --type zip"
        //            }
        //     }
        // }

    //      stage('Debug my-app Directory for publish file') {
    //       steps {
    //         dir('my-app') {
    //            bat 'dir'
    //        }
    //      }
    //    }

    //     stage('Deploy'){
            
    //         steps{
    //             dir('my-app'){
    //                 //bat "az webapp deploy source config-zip --resource-group $RESOURCE_GROUP --name $APP_SERVICE_NAME --src-path ./publish.zip --type zip"
    //                  bat "if exist ./publish.zip echo 'File exists'"
    //                 bat "az webapp deploy -g $RESOURCE_GROUP -n $APP_SERVICE_NAME --src-path C:/ProgramData/Jenkins/.jenkins/workspace/reactPipeline/my-app/publish.zip --type zip"
    //             }
    //         }
    //     }
    // }
        stage('Zip Build Folder') {
      steps {
        dir("${REACT_APP_DIR}\\build") {
          bat 'powershell Compress-Archive -Path * -DestinationPath ..\\build.zip -Force'
        }
      }
    }

    stage('Deploy to Azure App Service') {
       steps {
      //       withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
      //       bat "az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID"

        dir("${REACT_APP_DIR}") {
          bat " az webapp deployment source config-zip --resource-group %AZURE_RG%  --name %AZURE_WEBAPP_NAME%  --src build.zip"
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
