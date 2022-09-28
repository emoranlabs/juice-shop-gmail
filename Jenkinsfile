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
        }/*
        stage('CNP Scan'){
            steps {
                 fortiCWPScanner imageName: 'juice-shop:latest', block: true
            }
        }*/
        stage('SAST'){
            steps {
                 sh 'docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
                 sh 'docker run --rm --mount type=bind,source="$PWD",target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
            }
        }
        stage('Push') {
            steps {
                script{
                        docker.withRegistry('https://363412468025.dkr.ecr.us-east-2.amazonaws.com/juice-shop-gmail', 'ecr:us-east-2:emoran') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                 sh 'kubectl apply -f deployment.yml'
            }
        }/* 
        stage('DAST'){
            steps {
                 sh 'sleep 1m'
                 sh 'docker pull registry.fortidevsec.forticloud.com/fdevsec_dast:latest'
                 sh 'docker run --rm --mount type=bind,source="$PWD",target=/scan registry.fortidevsec.forticloud.com/fdevsec_dast:latest'                 
            }*/
        }
    }
}
