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

    }
}