pipeline{
   agent{
    // running on  the agent
      node{
        label 'agent-1'
      }
   }

    stages{
        stage('build'){
            steps{
                echo "it is building stage"
            }
        }

        stage('test'){
            steps{
                echo "it is testing stage"
            }
        }

        stage('deploy'){
            steps{
                echo "it is deply phase"
            }
        }
    }
}