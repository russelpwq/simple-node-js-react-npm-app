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
                // Run the deliver script
                sh './jenkins/scripts/deliver.sh'
                
                // Wait for user input before proceedin
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                
                // Run the kill script
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

    post {
        always {
            emailext(
                to: 'russelpoon25@gmail.com',
                subject: "Build ${currentBuild.fullDisplayName}",
                body: "The build ${currentBuild.fullDisplayName} has completed.\n\n" +
                      "Status: ${currentBuild.result}\n" +
                      "Build URL: ${env.BUILD_URL}"
            )
        }
    }
}
