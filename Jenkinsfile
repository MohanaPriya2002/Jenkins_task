pipeline{
    agent any

    environment {
        DIRECTORY_PATH = "https://github.com/MohanaPriya2002/Jenkins_task/edit/main/Jenkinsfile"
        TESTING_ENVIRONMENT='TESTING'
        PRODUCTION_ENVIRONMENT= 'LAKSHMI MOHANA PRIYA MADDULA'
        EMAIL_RECIPIENT = 'mmohana923@gmail.com'
    }

    stages{
        stage('Build'){
            steps{
                echo "Fetching the source code from the directory path specified by the environment variable"
                echo "Compiling the code and generating any necessary artifacts"
            }
        }
        stage('Test'){
            steps{
                echo "Running the unit tests"
                echo "Running the integration tests"
            }
            post{
                always{
                    script{
                        def testStatus = currentBuild.currentResult
                        echo "Sending email"
                        emailext (
                            to: "${EMAIL_RECIPIENT}",
                            subject: "Unit and Integration Tests Stage: ${testStatus}",
                            body: "The Unit and Integration Tests stage has finished with status: ${testStatus}",
                            attachmentsPattern: '**/target/surefire-reports/*.xml'
                        )
                    }
                }
            }
        }
        stage('Code Analysis'){
            steps{
                echo "Running code analysis using pmd"
            }
        }
        stage('Security Scan') {
            steps {
                echo "Performing security scan using Snyk"
            }
            post {
                always {
                    script {
                        def scanStatus = currentBuild.currentResult
                        echo "Sending email"
                        emailext (
                            to: "${EMAIL_RECIPIENT}",
                            subject: "Security Scan Stage: ${scanStatus}",
                            body: "The Security Scan stage has finished with status: ${scanStatus}",
                            attachmentsPattern: '**/dependency-check-report.xml'
                        )
                    }
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                echo "Deploying the application to the Staging Server"
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests on the staging environment"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying the application to the production environment (AWS EC2 instance)"
            }
        }
    }
}
