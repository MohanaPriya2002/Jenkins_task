pipeline{
    agent any

    environment {
        DIRECTORY_PATH = "C:/Users/mmoha/Downloads/SIT-753--Task"
        TESTING_ENVIRONMENT='TESTING'
        PRODUCTION_ENVIRONMENT= 'LAKSHMI MOHANA PRIYA MADDULA'
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
        }
        stage('Code'){
            steps{
                echo "Check the quality of the code"
            }
        }
        stage('Deploy'){
            steps{
                echo "Deploying the application to a testing environment specified by the environment variable"
            }
        }
        stage('Approval'){
            steps{
                echo "Getting approval from the stakeholders"
            }
        }
        stage('Deploy to Production'){
            steps{
                echo "Deploying the application to the production environment"
            }
        }
    }
}