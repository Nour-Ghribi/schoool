pipeline {
  agent any

  environment {
    IMAGE = "nourxgh/school:1.0.0"
    DOCKERHUB_CRED = "dockerhub-conn"   //  credential existant
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }

    stage('Push Docker Image to DockerHub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: "${DOCKERHUB_CRED}",
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push $IMAGE
          '''
        }
      }
    }

    stage('Deploy (Docker Compose)') {
      steps {
        sh 'docker compose up -d'
      }
    }
  }
}
