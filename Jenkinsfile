pipeline{
    agent any
    stages{
        stage("Start Grid"){
            steps{
                sh "docker-compose --profile grid up -d"
            }
        }
        stage("Run Tests"){
            steps{
                sh "docker-compose --profile test up"
            }
        }
        stage("Stop Grid"){
            steps{
                sh "docker-compose down"
            }
        }
    }
}