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
       //   withKubeConfig([credentialsId: 'k8config']) {
          //sh("kubectl apply -f auth.yaml -v=10")
          sh("kubectl apply -f service.yaml -v=10")
          sh("kubectl apply -f deployment.yaml -v=10")
          //withKubeConfig(configs: "deployment.yaml", "service.yaml")
         // }
        }
      }
    }

  }

}
