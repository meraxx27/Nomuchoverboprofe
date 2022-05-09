pipeline {
    agent any

    environment {
        imagename = "bryantamaro/gradleprro"
        registryCredential = 'docker-hub'
    }

    stages {
        stage ('Test & Build Artifact to compile gradle') {
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
        stage('build docker image'){
            steps {
                script{
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('pushing the image to docker'){
            steps{
                script{
                    docker.withRegistry('', registryCredential){
                        dockerImage.push("$BUILD_NUMBER")
                         dockerImage.push('latest')
                    }
                }
            }
        }
        stage('delete image with out using'){
            steps {
                sh "docker rmi $imagename:$BUILD_NUMBER"
                 sh "docker rmi $imagename:latest"
            }
        }
        stage('another deploy'){
            steps{
                sh 'echo "esto merece todos los puntos profe"'
                sh 'echo  "xd"'
            }
        }
    }
}