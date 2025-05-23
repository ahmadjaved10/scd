pipeline {
  agent any
  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/ahmadjaved10/scd.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          docker.build('node-app')
        }
      }
    }
    stage('Push to Docker Hub') {
      steps {
        withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
          script {
            docker.image('kub').push('latest')
          }
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }
}
