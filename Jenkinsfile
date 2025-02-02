pipeline {
    agent any

        environment {
        JAVA_HOME = 'C:/Program Files/Java/jdk-17' // Ensure this matches your JDK path
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
        SONAR_TOKEN = credentials("sonar-token")
            
        }
    tools {
        // Install the Maven version configured as "M2" and add it to the path.
        maven "MAVEN"
    }

    stages {
           stage('Sonar Scan') {
            steps {
                sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=org-mahendra_book-api"
                
            }
           }

        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/mahendra-shinde/sample-library-api/'
                sh "mvn  clean package"
                // To run Maven on a Windows agent, use
                //bat "mvn  clean package"
                
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                        always {
            echo 'Cleaning up...'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
