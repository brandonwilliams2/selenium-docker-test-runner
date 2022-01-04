pipeline{
    agent any
    stages{
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
        stage("Stop Grid"){
            steps{
                sh "docker-compose down"
            }
        }
    }
}