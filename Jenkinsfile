pipeline {
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
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
    // Seperate Stage for Docker file multi-stage, it is building image and pushing it to Nexus repo
    stage("Build Docker images and push to Nexus"){
        steps{
            script{
                withCredentials([string(credentialsId: 'nexus_pass', variable: 'nexus_pass_var')]) {
                sh '''
                    docker build -t 192.168.56.103:8083/javawebapp:${VERSION} .
                    docker login -u admin -p $nexus_pass_var 192.168.56.103:8083
                    docker push 192.168.56.103:8083/javawebapp:${VERSION}
                    docekr rmi 192.168.56.103:8083/javawebapp:${VERSION}
                '''
                }

            }
        }
    }
    // last part
    post{ 
        success{
            echo "=== Executed successfully =="
        }
        failure{
            echo "==== Failed execution ====="
        }
    }
}