pipeline{
    agent any
    stages{
        stage("Start Grid"){
            steps{
                sh "docker-compose up -d hub chrome firefox"
            }
        }
        stage("Run Tests"){
            steps{
                sh "docker-compose up search-feature book-flight-feature"
            }
        }
        stage("Stop Grid"){
            steps{
                sh "docker-compose down"
            }
        }
    }
}
