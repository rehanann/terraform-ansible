pipeline {
  agent any
  environment {
    SVC_ACCOUNT_KEY = credentials('terraform-auth')
  }
  stages {
            stage('Checkout') {
                steps {
                    checkout scm
                    sh 'echo $SVC_ACCOUNT_KEY | base64 -d > serviceaccount.json'
                    sh 'cat /var/lib/jenkins/mytest-secrets/providers.tf > provides.tf'
                    sh 'cat /var/lib/jenkins/mytest-secrets/variables.tf > variables.tf'
                    // sh '/root/secrets/providers.tf .'
                    // sh '/root/secrets/variables.tf .'
                    sh 'cat variables.tf'
                    sh 'cat providers.tf'
                    }
             }
            stage('TF Plan') {
                steps {
                sh 'ls -l'
                sh 'pwd'
                // sh 'cd first_test'
                sh 'terraform init'
                sh 'terraform plan -out myplan'
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
                    sh 'terraform apply myplan -auto-approve'
            }
        }
    }
}