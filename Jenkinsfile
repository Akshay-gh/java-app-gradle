pipeline {
    agent any
    environment{
        VERSION = "{env.BUILD_ID}"
    }
    stages{
        stage("Sonar Quality Check"){
            steps{
                script{
                    // SonarQube integration
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube --info'
                    }// After Adding webhook checked for quality gate status
                    timeout(time: 5, unit: 'MINUTES'){
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK')
                        error "Pipeline aborted due to quality gate failure ${qg.status}"
                    }
                }
            }
        }

    }
    post{ // last part
        success{
            echo "=== Executed successfully =="
        }
        failure{
            echo "==== Failed execution ====="
        }
    }
}