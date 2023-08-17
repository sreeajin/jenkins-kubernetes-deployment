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
          sh("kubectl apply -f deployment.yaml --context kubernetes")
           sh("kubectl apply -f service.yaml --context kubernetes")
          //withKubeConfig(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
