pipeline {
    agent any
    tools {
	    maven "MAVEN3"
	    jdk "OracleJDK11"
	}

    stages{
        stage('Fetch code') {
          steps{
              git branch: 'main', url:'https://github.com/PranitRout07/java-docker-genie.git'
          }  
        }

        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
        stage('docker build and deploy'){
            steps {
                bat 'docker build -t java-web-app .'
            }
            
        }
        stage('Push Docker image to Docker Hub'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                   bat "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                   bat "docker tag java-web-app ${env.dockerUser}/java-web-app:latest"
                   bat "docker push ${env.dockerUser}/java-web-app:latest "
                }
            }
        }
        stage('Run on docker'){
            steps {
                echo "running docker image at port 8080"
                bat "docker run -d -p 80:8080 pranit007/java-web-app:latest"
            }
        }
    }
}
