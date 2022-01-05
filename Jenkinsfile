pipeline{
    agent any
    stages{
        stage("Pull Latest Image"){
            steps{
                sh "docker pull brandonwilliams2/selenium-docker"
            }
        }
        stage("Start Grid"){
            steps{
                sh "docker-compose --profile grid up -d --no-color"
            }
        }
        stage("Run Tests"){
            steps{
                sh "docker-compose --profile test up --no-color"
            }
        }
    }
    post{
        always{
            archiveArtifacts artifacts: 'output/**'
            sh "docker-compose --profile grid down"
            sh "docker-compose --profile test down"
        }
    }
}