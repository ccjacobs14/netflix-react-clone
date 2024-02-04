pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("698276559677.dkr.ecr.eu-west-2.amazonaws.com/netflix:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 698276559677.dkr.ecr.us-east-1.amazonaws.com/netflix:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://698276559677.dkr.ecr.us-east-1.amazonaws.com/netflix', 'ecr:us-east-1:14AWS') {
                    // build image
                    def myImage = docker.build("698276559677.dkr.ecr.us-east-1.amazonaws.com/netflix:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
