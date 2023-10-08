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
                    // Set a timeout of 5 minutes for the entire stage
                    timeout(time: 5, unit: 'MINUTES') {
                        // Wait for the SonarQube quality gate result and abort the pipeline if it fails
                        waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonarqube-token'      
                    }
                    // Retrieve the quality gate status
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                
                        // Check the status and raise an error if it's not 'OK'
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to Sonar quality gate failure: ${qg.status}"
                        }

                }
            }
        }

    }
}    
