// Jenkinsfile used to build an image of a node app, login to DockerHub and push the image to the repo

//1. Checkout the code from the Source Code Management
//2. Build the Docker image
//3. Login to Dockerhub with the valid credentials
//4. Push the image onto the online Docker repository

pipeline {
    agent any          // can run on any agent (master or slave)
    environment {
        DOCKERHUB_CREDENTIALS = credentials('vedantd0cker')      // environment variables
    }
    // the stages of the pipeline start
    stages {
        // individual stages 
        stage('SCM Checkout code') {
            // steps to be executed inside each stage
            steps{
            git 'https://github.com/Vedant-MAHAjan/Docker-Jenkins.git'
            }
        }

        stage('Build docker image urgent') {
            steps {  
                // since it is running on Windows, use "bat" instead of "sh"
                bat 'docker build -t nodeapp .'
            }
        }
        stage('Login to Dockerhub') {
            // withCredentials is used to fetch the credentials of Docker saved into Jenkins 
            steps{
                withCredentials([usernamePassword(credentialsId: 'vedantd0cker', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                bat "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                bat "docker tag nodeapp vedantd0cker/nodeapp:latest"               
                }
            }
        }
        stage('Push the Image') {
            steps{
                bat "docker push vedantd0cker/nodeapp:latest"
            }
        }
    }
//post actions, which will always be executed, no matter the result of previous stages
// usually used to perform cleanups of the workspace, logging out, etc    
post {
        always {
            bat 'docker logout'
        }
    }
}
