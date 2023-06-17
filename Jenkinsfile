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
                    dockerImage = docker.build registry + ":latest"
                }
            }
        }
        stage('Delivery image') {
            steps{
                script {
                  docker.withRegistry('https://registry-1.docker.io/v2/', 'dockerhub') {
                   dockerImage.push("latest")
                  }
                }
            }
        }
        stage ('Fazer o deploy em produção ?') {
            steps {
            input (
                message: "Deseja fazer deploy em produção ?",
                ok: "Sim"
            )
                script {
                    sh "sudo docker rm developer -f"
                    sh "sudo docker run -d --name developer -p 3030:80 denisdbp/mobead_image_build:latest" 
                }
            }
        }
    } 
}