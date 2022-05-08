pipeline {
    agent any

    environment {
        imagename = "bryantamaro/gradleprro"
        registryCredential = 'docker-hub'
    }

    stages {
        stage ('Test & Build Artifact') {
            agent {
                docker {
                    image 'openjdk:11'
                    args '-v "$PWD":/app'
                    reuseNode true
                }
            }
            steps {
                sh "chmod +x gradlew"
                sh './gradlew clean build'
            }
        }
        stage('Construir imagen'){
            steps {
                script{
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('Hacer deploy de imagen'){
            steps{
                script{
                    docker.withRegistry('', registryCredential){
                        dockerImage.push("$BUILD_NUMBER")
                         dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Eliminar imagen sin usar'){
            steps {
                sh "docker rmi $imagename:$BUILD_NUMBER"
                 sh "docker rmi $imagename:latest"
            }
        }
    }
}