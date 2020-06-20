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
        stage('kubernates deploy'){
            sh "chmod +x changeTag.sh"
            sh "./changeTag.sh ${DOCKER_TAG}"
            sshagent(['kops-machine']) {
              sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ec2-user@52.23.245.23:/home/ec2-user/"
                script{
                    try{
                       sh "ssh ec2-user@52.23.245.23 kubectl apply -f ." 
                        
                    }catch(error){
                        sh "ssh ec2-user@52.23.245.23 kubectl create -f ." 
                    }
                    
                }
            }
        }
   }
}   

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
