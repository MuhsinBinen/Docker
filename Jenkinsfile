pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[url: 'https://github.com/MuhsinBinen/Docker']]
                )
                bat 'mvn clean install'
            }
        }
 	stage('Stop and Remove Existing Container') {
             steps {
                 script {

                    bat 'docker stop demo-container '
                    bat 'docker rm demo-container'
                 }
              }
        }
        stage('Build docker image'){
            steps{
                script{
                    docker.build("demo13:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    docker.image("demo13:${env.BUILD_NUMBER}").run("-d -p 8083:8083 --name demo-container")
                }
            }
  }
}

}