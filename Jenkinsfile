pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Define SonarQube scanner tool
                SONARQUBE_SCANNER_HOME = tool 'SonarScanner'
            }
            steps {
                script {
                    // Run SonarQube Scanner with sonar.projectKey
                    withSonarQubeEnv() {
                        sh """
                            ${SONARQUBE_SCANNER_HOME}/bin/sonar-scanner 
                            -Dsonar.projectKey=sonar.projectKey=test-project-jenkins-key3 
                            -Dsonar.projectName=test-project-jenkins 
                            -Dsonar.sources=.
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube analysis and check Quality Gate status
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }

        // Add more stages as needed for your pipeline
    }
}
