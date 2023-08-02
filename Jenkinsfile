pipeline {
  agent any
  environment {
    IMAGE_NAME='deb'
    DOCKERFILE_PATH='debasisgmail/cosigntest'
    IMAGE_VERSION='latest'
    IMAGE_TAG='v2'
    //DOCKER_CREDENTIALS=credentials('docker-credentials')
    COSIGN_PASSWORD=credentials('cosign-password')
    COSIGN_PRIVATE_KEY=credentials('cosign-private-key')
    COSIGN_PUBLIC_KEY=credentials('cosign-public-key')
    
  }
  stages {
      stage('cleanup') {
      steps {
        sh 'docker system prune -a --volumes --force'
      }
    }   
    stage('build image') {
      steps {
          sh 'docker build -t $DOCKERFILE_PATH:$IMAGE_VERSION .'
      }
    }
 
    stage('login to Docker') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh 'docker login -u $USERNAME -p $PASSWORD'
        }
      }       
    }
    stage('tag image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker tag $DOCKERFILE_PATH:$IMAGE_VERSION $USERNAME/$IMAGE_NAME:$IMAGE_TAG'
          }
      }
    }
    stage('push image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker push $USERNAME/$IMAGE_NAME:$IMAGE_TAG'
          }
      }
    }
    stage('sign the container image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'cosign version'
              sh 'cosign sign --key $COSIGN_PRIVATE_KEY $USERNAME/$IMAGE_NAME:$IMAGE_TAG -y'
              
          }
      }
    }

    stage('verify the container image') {
      steps {
          sh 'cosign verify --key $COSIGN_PUBLIC_KEY debasis12345/$IMAGE_NAME:$IMAGE_TAG'
      }
    }
  } 
}
