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
                sh './gradlew assemble'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Build and push Docker image') {
            docker.withRegistry('https://registry.example.com', 'credentials-id') {
                /* Build the image */
                def customImage = docker.build("spring-boot-api-example:latest", "--build-arg JAR_FILE=build/libs/spring-boot-api-example-0.1.0-SNAPSHOT.jar .")
                /* Push the container to the custom Registry */
                customImage.push()
            }
        }
    }
}