//echo "tetsing"
pipeline {
  agent any
  environment {
    //GITHUB_TOKEN=credentials('debasisjenkins')
    IMAGE_NAME='debasisgmail/cosigntest'
    IMAGE_VERSION='8.5-204-v1'
    //DOCKER_CREDENTIALS=credentials('docker-credentials')
    COSIGN_PASSWORD=credentials('cosign-password')
    COSIGN_PRIVATE_KEY=credentials('cosign-private-key')
    
  }
  stages {
    stage('build image') {
      steps {
          sh 'whoami'
          sh 'docker ps -a'
          sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
      }
    }
 
    stage('login to Docker') {
      steps {
       //withCredentials([usernamePassword(credentialsId: "$docker-credentials", passwordVariable: '', usernameVariable: '')])
        sh 'docker login --username debasis12345 --password Chakuli@123456'
      }

       
    }
    stage('tag image') {
      steps {
        sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION debasis12345/deb:v4'
      }
    }
    stage('push image') {
      steps {
        sh 'docker push debasis12345/deb:v4'
      }
    }
    stage('sign the container image') {
      steps {
        //withCredentials([usernamePassword(credentialsId: "$cosign_key", passwordVariable: '', usernameVariable: '')])
        sh 'cosign version'
        //sh 'cosign sign --key $COSIGN_PRIVATE_KEY ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
        sh 'cosign sign --key $COSIGN_PRIVATE_KEY debasis12345/deb:v4 -y'
      }
    }
  } 
}
