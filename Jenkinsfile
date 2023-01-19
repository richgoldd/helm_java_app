pipeline {
  agent any
  stages {      
        stage('Github checkout') {
            steps {
              echo 'Checking out github repo'
              checkout scm
              }
            }
        stage('Build maven') {
            agent {
              docker {
                   image 'maven:3.8.7-sapmachine-11'
                    }
               }
            steps { 
                    sh 'pwd'      
                    sh 'mvn  clean install package'
            }
          }
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {         
                 def customImage = docker.build('richgold/petclinic', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
    }
}
