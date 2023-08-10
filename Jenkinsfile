pipeline {
    agent any

    stages{
        stage("sonar Quality Check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }

    }
    post{
        success{
            echo "=== Executed successfully =="
        }
        failure{
            echo "==== Failed execution ====="
        }
    }
}