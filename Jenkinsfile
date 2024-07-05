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
                            -Dsonar.projectKey=4fb111d682327ac0f2e5e09d4f78e8890549c3dc 
                            -Dsonar.projectName=TestProject-Jenkins 
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
