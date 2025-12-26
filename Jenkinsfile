pipeline {
    agent {
        node{
            label 'agent-1'
        }
    }

    environment(){
      COURSE ="jenkins"
    }

    options {
        timeout(time: 10, unit: 'SECONDS') 
    }
      // Testing the web hook
      parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Build') {
            steps {
                script{
                    sh """
                        echo "Building"
                        echo $COURSE
                        #env
                        #sleep 10 
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
                input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
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
            aborted{
                echo "this case is aborted"
            }
        }
}