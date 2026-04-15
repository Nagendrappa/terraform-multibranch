pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {

        stage('Terraform Init') {
            steps {
                sh 'terraform init -no-color'
            }
        }

        stage('Terraform Validate') {
            when {
                branch 'main'
            }
            steps {
                sh 'terraform validate -no-color'
            }
        }

        stage('Terraform Plan') {
            when {
                branch 'main'
            }
            steps {
                sh 'terraform plan -no-color'
            }
        }

        stage('Terraform Apply') {
            when {
                branch 'main'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {

                    sh 'terraform apply -auto-approve -no-color'

                }
            }
        }

        stage('Terraform Destroy') {
            when {
                branch 'destroy'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {

                    sh 'terraform destroy -auto-approve -no-color'

                }
            }
        }

    }

    post {
        always {
           // cleanWs()
        }
    }
}
