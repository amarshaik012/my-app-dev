pipeline {
  agent any

  environment {
    KUBECONFIG = '/root/.kube/config'  // kubeconfig path inside Jenkins container
    IMAGE_NAME = "my-app-dev:latest"
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
          dockerImage = docker.build(env.IMAGE_NAME)
        }
      }
    }

    // Optional: Docker login to push image (uncomment and configure if needed)
    /*
    stage('Docker Login & Push') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            dockerImage.push()
          }
        }
      }
    }
    */

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh '''
            echo "Deploying to Kubernetes..."
            kubectl version --client
            kubectl apply -f k8s/deployment.yaml || exit 1
            kubectl apply -f k8s/service.yaml || exit 1
          '''
        }
      }
    }
  }
}
