pipeline{
    agent any
    stages{
        stage("Start Grid"){
            steps{
                sh "docker ps"
            }
        }
        stage("Run Tests"){
            steps{
                sh "docker-compose --profile test up --no-colors"
            }
        }
        stage("Stop Grid"){
            steps{
                sh "docker-compose down"
            }
        }
    }
}
