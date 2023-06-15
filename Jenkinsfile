pipeline{
  environment {
    registry = "phpstacktest/nodejs-helloworld"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
    stages {
        stage('Build'){
            steps{
                script{
                    bat 'npm install'
                }
            }
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":latest"
                }
             }
          }
          stage('Push Image') {
              steps{
                  script 
                    {
                        docker.withRegistry( '', registryCredential ) {
                            dockerImage.push('latest')
                        }
                   } 
               }
            }
        stage('Deploying into k8s'){
            steps{
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    bat 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
