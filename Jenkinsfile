//echo "tetsing"


pipeline {
  agent any
  environment {
    GITHUB_TOKEN=credentials('debasisjenkins')
    IMAGE_NAME='debasisgmail/cosigntest'
    IMAGE_VERSION='8.5-204-v1'
    //DOCKER_CREDENTIALS=credentials('dockercredentials')
    
  }
  stages {
    stage('build image') {
      steps {
        sh 'sudo docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
      }
    }
 
    stage('login to Docker') {
      steps {
        sh 'docker login -u debasis12345 -p Cha'
      }

       
    }
    stage('tag image') {
      steps {
        sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION debasis12345/$IMAGE_NAME:$IMAGE_VERSION'
      }
    }
    stage('push image') {
      steps {
        sh 'docker push debasis12345/$IMAGE_NAME:$IMAGE_VERSION'
      }
    }
  } 
}
