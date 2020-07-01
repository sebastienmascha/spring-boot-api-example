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
                sh 'java -version'
                sh '/usr/libexec/java_home -V'
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