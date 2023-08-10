pipeline {
    agent any
    environment{
        VERSION = '{env.BUILD_ID}'
    }

    stages{
        stage("sonar Quality Check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube --info'
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