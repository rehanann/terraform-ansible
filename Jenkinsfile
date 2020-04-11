pipeline {
  agent any
  stages {
      stage('authentiction') {
          steps {
              SVC_ACCOUNT_KEY = credentials('provides')
          }
      }
  }
  environment {
    SVC_ACCOUNT_KEY = credentials('terraform-auth')
  }
  }
  stages {
            stage('Checkout') {
                steps {
                    // checkout scm
                    sh 'echo $provides | base64 -d > first_test/provides.tf'
                    sh 'echo $variables | base64 -d > first_test/variables.tf'
                    sh 'cat first_test/provides.tf'
                    sh 'cat first_test/variables.tf'
                    }
             }
            stage('TF Plan') {
                steps {
                container('terraform') {
                sh 'cd first_test'
                sh 'terraform init'
                sh 'terraform plan -out myplan'
                }
            }
        }
        // stage('Approval') {
        //         steps {
        //             script {
        //              def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
        //         }
        //     }
        // }
        stage('TF Apply') {
                steps {
                    container('terraform') {
                    sh 'terraform apply myplan -auto-approve'
                
                }
            }
        }
    }
}