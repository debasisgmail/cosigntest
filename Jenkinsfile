//echo "tetsing"


pipeline {
  agent any
  environment {
    //GITHUB_TOKEN=credentials('debasisjenkins')
    IMAGE_NAME='debasisgmail/cosigntest'
    IMAGE_VERSION='8.5-204-v1'
    //DOCKER_CREDENTIALS=credentials('dockercredentials')
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
        sh 'docker login -u debasis12345 -p Chakuli@123456'
      }

       
    }
    stage('tag image') {
      steps {
        sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION debasis12345/deb:v3'
      }
    }
    stage('push image') {
      steps {
        sh 'docker push debasis12345/deb:v3'
      }
    }
    stage('sign the container image') {
      steps {
        //withCredentials([usernamePassword(credentialsId: "$cosign_key", passwordVariable: '', usernameVariable: '')])
        sh 'cosign version'
        //sh 'cosign sign --key $COSIGN_PRIVATE_KEY ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
        sh 'cosign sign --key $COSIGN_PRIVATE_KEY debasis12345/deb:v2'
        //sh 'cosign sign --key $COSIGN_PRIVATE_KEY debasis12345/deb:v3'
      }
    }
  } 
}
