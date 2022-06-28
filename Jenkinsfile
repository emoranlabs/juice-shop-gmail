pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("juice-shop")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Nada aqui'
            }
        }
        stage('Deploy') {
            steps {
                script{
                        docker.withRegistry('https://371571523880.dkr.ecr.us-east-1.amazonaws.com/juice-shop', 'ecr:us-east-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
