pipeline {
    agent any
    stages{
        stage('Build Docker Image'){
            steps{
                bat "docker build . -t nk95/nodeapp22"
                
            }
        }
   }
}   
