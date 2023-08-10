pipeline {

  environment {
    dockerimagename = "laks888/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script { 
          sh("minikube kubectl apply -f deployment.yaml")
           sh("minikube kubectl apply -f service.yaml")
          //withKubeConfig(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
