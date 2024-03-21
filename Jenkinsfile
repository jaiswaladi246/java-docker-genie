pipeline {
    agent any
    tools {
        maven 'maven3'
    }

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/repo19352/java-docker-genie.git'
            }
        }
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
    }
}
