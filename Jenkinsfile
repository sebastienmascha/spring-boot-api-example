pipeline {
    agent any

    environment {
        DOCKER_HUB = credentials('docker-hub')
    }

    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage("build and test the project") {
            agent {
                docker { image 'openjdk:11' }
            }
            stages {
                stage('Build') {
                    steps {
                        sh './gradlew assemble'
                    }
                }
                stage('Test') {
                    steps {
                        sh './gradlew test'
                    }
                }
            }
        }
        stage('Build and push Docker image') {
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                script {
                    /* Build the image */
                    def customImage = docker.build("smascha/spring-boot-api-example:latest", "--build-arg JAR_FILE=build/libs/spring-boot-api-example-0.1.0-SNAPSHOT.jar .")
                    /* Push the container to the custom Registry */
                    customImage.push()
                }
            }
        }
    }
}