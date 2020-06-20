pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t nk95/nodeapp:${DOCKER_TAG}"
            }
        }
        stage('push image docker'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerhubpwd')]) {
                    sh "docker login -u nk95 -p ${dockerhubpwd}"
                    sh "docker push nk95/nodeapp:${DOCKER_TAG}"
                }
            }
        }
   }
}   

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
