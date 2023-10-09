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
                    def scannerHome = tool 'Sonarqube'
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    sleep(60)
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonarqube-token'
                    }
                }
            }
        }
        
    }
}


    }
}    
