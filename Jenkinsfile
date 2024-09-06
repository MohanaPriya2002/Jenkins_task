pipeline{
    agent any

    tools{
        maven 'Maven'
        snyk 'Snyk'
    }

    environment {
        DIRECTORY_PATH = "https://github.com/MohanaPriya2002/Jenkins_task/edit/main/Jenkinsfile"
        TESTING_ENVIRONMENT='TESTING'
        PRODUCTION_ENVIRONMENT= 'LAKSHMI MOHANA PRIYA MADDULA'
        EMAIL_RECIPIENT = 'mmohana923@gmail.com'
        ANCHORE_URL= 'http://your_anchore_enterprise_host_ip:your_anchore_enterprise_port/v2'
    }

    stages{
        stage('Build'){
            steps{
                echo "Fetching the source code from the directory path specified by the environment variable"
                echo "Compiling the code and generating any necessary artifacts"
                bat 'mvn clean install -X'
            }
        }
        stage('Test'){
            steps{
                echo "Running the unit tests"
                echo "Running the integration tests"
                bat 'mvn test'
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
                echo "Running code analysis using SonarQube"
                bat 'mvn pmd:pmd'
            }
        }
        stage('Security Scan') {
            steps {
                echo "Performing security scan using OWASP Dependency-Check"
                script {
            // Pull the OWASP ZAP Docker image if using Docker
            bat 'docker pull owasp/zap2docker-stable'
            
            // Run OWASP ZAP security scan
            bat 'docker run --rm -v $(pwd):/zap/wrk/:rw owasp/zap2docker-stable zap-baseline.py -t <your-app-url> -r zap-report.html'

            // Save the report as an artifact
            archiveArtifacts artifacts: 'zap-report.html'
        }
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
                bat 'scp target/app.jar ec2-user@staging-server:/home/ec2-user/app/'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests on the staging environment"
                bat './run_staging_integration_tests.sh'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying the application to the production environment (AWS EC2 instance)"
                bat 'scp target/app.jar ec2-user@production-server:/home/ec2-user/app/'
            }
        }
    }
}
