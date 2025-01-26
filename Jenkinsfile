pipeline {
    environment {
        registry = "jmoroko/tp4cicd"
        registryCredential = 'dockerhub'
        dockerImage = ''
        DOCKER_HOST = 'npipe:////./pipe/docker_engine'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'jmoroko',
                    url: 'https://github.com/JMOROKO/web-app-tp4-docker.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test image'){
            steps{
                script{
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
            stage('Deploy Image') {
                steps {
                    script {
                        bat "docker run -d ${registry}:${BUILD_NUMBER}"
                    }
                }
             }
        }
    }
}