pipeline {
    agent any
    stages{

        stage("Git Test"){
            steps{
                echo "Executing git connectivity test"
            }
            post{
                success{
                    echo "git repo cloned successfully"
                }
                failure{
                    echo "git repo clone failed"
                }
            }
        }

        post{

            success{
                echo "=============== Pipeline Executed successfully ====================="
            }

            failure{
                echo "================= Pipeline execution failed ========================"
            }
        }
    }
}