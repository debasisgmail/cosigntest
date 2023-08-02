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
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh 'docker login -u $USERNAME -p $PASSWORD'
        }
        //sh 'docker login --username debasis12345 --password Chakuli@123456'
      }

       
    }
    stage('tag image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION $USERNAME/deb:v4'
          }
        //sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION $USERNAME/deb:v4'
      }
    }
    stage('push image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker push $USERNAME/deb:v4'
          }
        //sh 'docker push $USERNAME/deb:v4'
      }
    }
    stage('sign the container image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'cosign version'
              sh 'cosign sign --key $COSIGN_PRIVATE_KEY $USERNAME/deb:v4 -y'
              
          }
        //withCredentials([usernamePassword(credentialsId: "$cosign_key", passwordVariable: '', usernameVariable: '')])
       // sh 'cosign version'
        //sh 'cosign sign --key $COSIGN_PRIVATE_KEY ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
        //sh 'cosign sign --key $COSIGN_PRIVATE_KEY $USERNAME/deb:v4 -y'
      }
    }
  } 
}
