pipeline {
  agent {
    docker {
       image 'maven:maven:3.9.1-amazoncorretto-8'
          }
        }

       IMAGE_NAME = "product_service"
    
    stages {      
        stage('Git Checkout') {
            steps { 
                    checkout scm
                 }
               }      
              
        stage('Build Stage') {
           steps { 
                   echo 'Building stage for the app...'
                   sh 'mvn compile'
           }
        }

        stage('Test App') {
           steps {
                   echo 'Testing stage for the app...'
                   sh 'mvn test'
           }
        }

        stage('Packaging Stage') {
           steps {
                   echo 'Packaging stage for the app...'
                   sh 'mvn packagee'
           }
        }

        stage('Docker Image Build') {
            steps {
                sh "docker build -t product_service:{env.BUILD_NUMBER} ."
            }
        }        

        stage('Push Docker Image to ECR') {
            steps {
                withAWS(credentials: 'AWS_CREDENTIALS_ID', region: 'us-west-1') {
                    sh 'aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 634639955940.dkr.ecr.us-west-1.amazonaws.com'
                    sh "docker tag product_service:${env.BUILD_NUMBER} 634639955940.dkr.ecr.us-west-1.amazonaws.com/product_service:${env.BUILD_NUMBER}"
                    sh "docker push 634639955940.dkr.ecr.us-west-1.amazonaws.com/product_service:${env.BUILD_NUMBER}"
                 }
              }
           }       


  post {
     failure {
       mail to: 'richgoldd2@gmail.com',
            subject: "Failed pipeline: ${currentBuild.fullDisplayName}"
            body: "Pipeline failed for ${env.BUILD_URL}"
    
       }
