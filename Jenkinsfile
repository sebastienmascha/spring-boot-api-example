pipeline {
    agent any

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
            environment {
                DOCKER_HUB = credentials('docker-hub')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_USR --password=$DOCKER_HUB_PSW'
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