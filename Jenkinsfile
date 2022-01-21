pipeline{
    agent any
    stages{
        stage("Pull Latest Image"){
            steps{
                sh "docker pull brandonwilliams2/java-selenium"
            }
        }
        stage("Start Grid"){
            steps{
                sh "docker-compose up -d hub chrome firefox --no-color"
            }
        }
        stage("Run Tests"){
            steps{
                sh '''
                    docker-compose up search-feature book-flight-feature  --no-color"
                '''
            }
        }
    }
    post{
        always{
            archiveArtifacts artifacts: 'output/**'
            sh "docker-compose down"

        }
    }
}