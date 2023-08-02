pipeline {
  agent any
  environment {
    //GITHUB_TOKEN=credentials('debasisjenkins')
    IMAGE_NAME='debasisgmail/cosigntest'
    IMAGE_VERSION='8.5-204-v1'
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
          sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
      }
    }
 
    stage('login to Docker') {
      steps {
       //withCredentials([usernamePassword(credentialsId: "$docker-credentials", passwordVariable: '', usernameVariable: '')])
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh 'docker login -u $USERNAME -p $PASSWORD'
        }
      }       
    }
    stage('tag image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION $USERNAME/deb:v5'
          }
      }
    }
    stage('push image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'docker push $USERNAME/deb:v5'
          }
      }
    }
    stage('sign the container image') {
      steps {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh 'cosign version'
              sh 'cosign sign --key $COSIGN_PRIVATE_KEY $USERNAME/deb:v5 -y'
              
          }
      }
    }

    stage('verify the container image') {
      steps {
          sh 'cosign verify --key $COSIGN_PUBLIC_KEY debasis12345/deb:v5'
      }
    }
  } 
}
