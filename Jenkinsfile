

pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }

    stages {
        stage('Compile') {
            steps {
               sh "mvn package -Dmaven.test.skip"
            }
        }
        stage('Docker-Build') {
            steps {
               script{
                withDockerRegistry(credentialsId: 'DockerCred'){
                     sh 'docker build -t spencerxxxwb/ekart:v1 -f docker/Dockerfile.'
                    }
                }
            }
        }
        stage('Trivy') {
            steps {
               sh ' trivy image --format table -o report.html spencerxxxwb/ekart:v1 '
            }
        }
        stage('Docker push') {
            steps {
              script{
                  withDockerRegistry(credentialsId: 'DockerCred') {
                    sh " docker push spencerxxxwb/ekart:v1"
                }
              }
            }
        }
    }
}
