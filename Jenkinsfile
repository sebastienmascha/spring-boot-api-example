pipeline {
    agent {
        docker { image 'openjdk:11' }
    }

    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building without test"
                sh './gradlew assemble'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build --build-arg JAR_FILE="build/libs/spring-boot-api-example-0.1.0-SNAPSHOT.jar" -t spring-boot-api-example:latest .'
            }
        }
        stage('Push Docker image') {
            environment {
                DOCKER_HUB_LOGIN = credentials('docker-hub')
            }
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh 'docker push spring-boot-api-example:latest'
            }
        }
    }
}