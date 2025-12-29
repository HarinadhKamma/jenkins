pipeline{
   agent{
      node{
        label 'agent-1'
      }
   }

    stages{
        stage('build'){
            steps{
                script{
                    script sh """
                echo "it is build stage"
                """

                } 
            }
        }

        stage('test'){
            steps{
                script { sh """
                echo "it is testing stage"
                """
                }
            }
        }

        stage('deploy'){
            steps{
                script{ sh """
                echo "it is deply phase"
                """
            }
            }
        }
    }

    post{
        always{
                echo "workspace is clean"
                cleanWs()
            }
            success {
                echo "this is success"
            }
            failure{
                echo "this is failure"
            }
            aborted{
                echo "this is aborted"
            }
        }
    
}