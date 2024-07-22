pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Run the test script
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
                    }
                }
            }
        }

        stage('Deliver') { 
            steps {
                // Run the deliver scrip
                sh './jenkins/scripts/deliver.sh'
                
                // Wait for user input before proceeding
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                
                // Run the kill script
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

    post {
        always {
            // Publish SonarQube analysis report
            recordIssues(enabledForFailure: true, tool: [name: 'SonarQube'])
        }
    }
}
