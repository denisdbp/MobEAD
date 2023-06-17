pipeline {  
    environment {
      registry = "denisdbp/mobead_image_build"
      registryCredential = 'dockerhub'
      dockerImage = ''
    }
    agent any 
    stages { 
        stage('Lint Dockerfile'){ 
            steps{
                echo "Pipeline Usando Jenkinsfile"
                sh 'sudo docker run --rm -i hadolint/hadolint < Dockerfile'
            }
        }
        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Delivery image') {
            steps{
                script {
                  docker.withRegistry('https://registry-1.docker.io/v2/', 'dockerhub') {
                   dockerImage.push("$BUILD_NUMBER")
                  }
                }
            }
        }
        stage('Public image') {
            steps{
                script {
                    sh "sudo docker rm production -f"
                    sh "sudo docker run -d --name production -p 3000:80 denisdbp/mobead_image_build:$BUILD_NUMBER" 
                }
            }
        }
    } 
}