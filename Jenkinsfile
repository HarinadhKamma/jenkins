pipeline {
    agent {
        node{
            label 'agent-1'
        }
    }

    environment(){
      COURSE ="jenkins"
    }
    stages {
        stage('Build') {
            steps {
                script{
                    sh """
                        echo "Building"
                        echo $COURSE
                       """
                }
                
            }
           
        }
        stage('Test') {
            steps {
                script{
                    sh """
                    echo "Testing.."
                    """
                }
                
            
            }
        }
        stage('Deploy') {
            steps {
                script{
                    sh """
                    echo "Deploying...."
                  
                    """
                }
                
            }
        }
    }
        post{
            always{
                cleanWs()
            }
            success{
                echo "this is success case "
            }
            failure{
                echo "this is faile case"
            }
        }
}