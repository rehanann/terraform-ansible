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
                    sh 'cp /var/lib/jenkins/mytest-secrets/provider.tf  provider.tf'
                    sh 'cp /var/lib/jenkins/mytest-secrets/variable.tf  variable.tf'
                    sh 'cat variable.tf'
                    sh 'cat provider.tf'
                    }
             }
            stage('TF Plan') {
                steps {
                sh 'ls -l'
                sh 'pwd'
                sh 'terraform init'
                sh 'terraform plan -out myplan'
            }
        }
        stage('Approval') {
                steps {
                    script {
                     def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
                }
            }
        }
        stage('TF Apply') {
                steps {
                    sh 'terraform apply myplan'
            }
        }
        stage('OPENSHIFT Installation') {
                steps {
                    sh 'git clone -b release-3.11 --single-branch https://github.com/openshift/openshift-ansible.git'
                    sh 'cp /var/lib/jenkins/mytest-secrets/inventory.ini inventory.ini'
                    sh 'ansible-playbook -i inventory.ini nodes ansible-pb/config.yml'
                    sh 'ansible-playbook -i inventory.ini nodes ansible-pb/docker-storage-setup-ofs.yml'
                    sh 'ansible-playbook -i inventory.ini openshift-ansible/playbooks/prerequisites.yml'
                    sh 'ansible-playbook -i inventory.ini openshift-ansible/playbooks/deploy_cluster.yml'
                }
        }
    }
}