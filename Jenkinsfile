pipeline {
    agent {
        node{
            label 'agent-1'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }

        post{
            always{
                cleanws()
            }
            success{
                echo "this is success case "
            }
            failure{
                echo "this is faile case"
            }
        }
    }
}