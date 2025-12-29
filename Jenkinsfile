pipeline{
   agent{
      node{
        label 'agent-1'
      }
   }

   environment{
    COURSE = "devops"
    app_version =""
   }

    stages{
        stage('build'){
            steps{
                script{
                sh """
                echo "it is build stage"
                echo ${COURSE}
                """

                } 
            }
        }

        stage('read json file'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    app_version = packageJson.version
                    echo "app version : ${app_version}"
                }
            }
        }
        stage('Install dependencies'){
            steps{
                script{
                    sh """
                    npm install 
                    """
                }
            }
        }

        stage('build image'){
            steps{
                script{
                    sh """
                    docker build -t catalogue:v1 .
                    docker images
                    docker push catalogue:v1
                    """
                }
            }
        }

        stage('test'){
            steps{
                script {
                sh """
                echo "it is testing stage"
                """
                }
            }
        }

        stage('deploy'){
            steps{
                script{
                sh """
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