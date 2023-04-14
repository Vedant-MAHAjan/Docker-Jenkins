# Jenkinsfile: Push Node.js custom image to Docker Hub 🚀

#### This Jenkinsfile provides an example pipeline for building a Node.js app, creating a custom Docker image, and pushing it to Docker Hub.

1. We will attempt to simulate a small portion of the DevOps workflow in real-world scenarios.
2. It entails creating a NodeJS application with a large team of developers. 
3. The application must be containerized, and the image must be uploaded to the team's Dockerhub repository. 
4. Every time the source code is modified, the developers should be able to pull and work on the updated images.

## Prerequisites 🔧

Before running this pipeline, make sure you have:

- A Node.js application with a Dockerfile.
- A Docker Hub account.
- A Jenkins instance.

## Pipeline Overview 👩‍💻

- Checkout code from Git repository.
- Build Node.js application using npm install command.
- Build Docker image using Dockerfile.
- Push Docker image to Docker Hub.

## Stages of Jenkinsfile 💻

The stages of the Jenkinsfile are as follows :

1. Checkout the code from Source Code Management

2. Build the Docker image

3. Log in to Dockerhub using your valid credentials.

4. Push the image to the Dockerhub repository

5. Logout of Dockerhub

## Steps 📈

### 1. Checkout code from Git repository

```
stage('Checkout') {
  steps {
    checkout([$class: 'GitSCM',
      branches: [[name: '*/master']],
      userRemoteConfigs: [[url: 'https://github.com/YOUR-REPOSITORY.git']]
    ])
  }
}
```

In this step, the pipeline checks out the code from the specified Git repository.

### 2. Build Node.js application

```
stage('Build App') {
  steps {
    sh 'npm install'
  }
}
```

This step installs the application dependencies using npm install command.

### 3. Build Docker image

```
stage('Build Image') {
  steps {
    script {
      def customImage = docker.build("YOUR-DOCKERHUB-USERNAME/YOUR-IMAGE-NAME:${env.BUILD_NUMBER}")
      customImage.push()
    }
  }
}
```
This step builds the Docker image using the Dockerfile and pushes it to Docker Hub. 

Replace YOUR-DOCKERHUB-USERNAME and YOUR-IMAGE-NAME with your Docker Hub username and desired image name.

### 4. Push Docker image to Docker Hub

```
stage('Push Image') {
  steps {
    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
    script {
      def dockerImage = "YOUR-DOCKERHUB-USERNAME/YOUR-IMAGE-NAME:${env.BUILD_NUMBER}"
      sh "docker push $dockerImage"
    }
  }
}
```

This step logs in to Docker Hub using Docker Hub username and password and pushes the Docker image to Docker Hub. 

Replace **YOUR-DOCKERHUB-USERNAME** and **YOUR-IMAGE-NAME** with your Docker Hub username and desired image name.

Make sure to define **DOCKERHUB_USERNAME** and **DOCKERHUB_PASSWORD** as environment variables in your Jenkins instance.

## Snippet of a successful Jenkins Pipeline

![image](https://user-images.githubusercontent.com/88843623/231981467-a44895c9-75c7-48d9-b797-9bc599103347.png)

