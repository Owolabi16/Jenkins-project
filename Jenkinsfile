pipeline {
    agent { label 'jenkins-Agent'}
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    stages {

        stage("Checkout from SCM") {
            steps {
            git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Owolabi16/Jenkins-project'    
            }
        }

        stage("Build Application") {
            steps {
                sh 'mvn clean package'
            }
        }

        stage("Test Application") {
            steps {
                sh 'mvn test'
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                    sh 'mvn sonar:sonar'
                }   
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    def qualityGateStartTime = new Date().toInstant().toEpochMilli()

                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'

                    def qualityGateEndTime = new Date().toInstant().toEpochMilli()
                    def qualityGateDuration = qualityGateEndTime - qualityGateStartTime
                    echo "Quality Gate took ${qualityGateDuration / 1000} seconds to complete."
                }
            }
        }
    }
}