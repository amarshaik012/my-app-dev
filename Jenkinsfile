pipeline {
  agent any

  environment {
    KUBECONFIG = '/root/.kube/config'
  }

  stages {
    stage('Clone') {
      steps {
        git branch: 'main', url: 'https://github.com/amarshaik012/my-app-dev.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("my-app-dev:latest")
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh '''
            echo "Deploying to Kubernetes..."
            kubectl apply -f k8s/deployment.yaml
            kubectl apply -f k8s/service.yaml
          '''
        }
      }
    }
  }
}
