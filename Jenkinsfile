pipeline{
    agent any
    stages{
        stage("Start Grid"){
            steps{
                sh "docker-compose up -d hub chrome firefox --no-color"
            }
        }
        stage("Run Tests"){
            steps{
                sh "docker-compose up search- --no-color"
            }
        }
        stage("Stop Grid"){
            steps{
                sh "docker-compose down"
            }
        }
    }
}
