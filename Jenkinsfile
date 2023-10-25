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
        stage('Test'){
            steps {
                bat 'mvn test'
            }
        }
        stage('Code Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
                  withSonarQubeEnv('sonar') {
                      bat '%scannerHome%\\bin\\sonar-scanner -Dsonar.projectKey=java-docker-genie ' +
                        '-Dsonar.projectName=java-docker-genie ' +
                        '-Dsonar.projectVersion=1.0 ' +
                        '-Dsonar.sources=. ' +
                        '-Dsonar.java.binaries=target/classes '

              }
            }

        }
        stage('scan with trivy') {
            steps {
            
                bat "docker run --rm -v D:/trivy-report/:/root/.cache/ aquasec/trivy:0.18.3 filesystem ."
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
    post {
        always {
            script {
                def textColor
                switch (currentBuild.currentResult) {
                    case 'SUCCESS':
                        textColor = 'green';
                        break
                    default:
                        textColor = 'red';
                }

                emailext (
                    subject: "pipeline status: ${currentBuild.currentResult}",
                    body: """<html>
                                <body>
                                    <p><b>Project: ${JOB_NAME}</b></p>
                                    <p><b>Build Number: ${BUILD_NUMBER}</b></p>
                                    <p><b>Build URL: ${BUILD_URL}</b></p>
                                    <p style='color: ${textColor};'>Build Result: ${currentBuild.currentResult}</p>
                                </body>
                            </html>""",
                    to: 'pranitrout72@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html'
                )
            }
        }
    }
}