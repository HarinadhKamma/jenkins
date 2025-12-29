pipeline{
   agent{
      node{
        label 'agent-1'
      }
   }

    stages{
        stage('build'){
            steps{
                script sh """
                echo "it is building stage"
                """
            }
        }

        stage('test'){
            steps{
                script sh """
                echo "it is testing stage"
                """
            }
        }

        stage('deploy'){
            steps{
                script sh """
                echo "it is deply phase"
                """
            }
        }
    }

    post{
        always{
            cleanWs(){
                echo "workspace is clean"
            }
            success {
                echo "this is success"
            }
            failure{
                echo "this is failure"
            }
        }
    }


}