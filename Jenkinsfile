pipeline{
   agent{
      node{
        label 'agent-1'
      }
   }

   environment{
    COURSE = "devops"
    app_version =""
    account_id = "349727115914"
    project ="roboshop"
    component = "catalogue"
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
                    withAWS(credentials:'aws-cred') {
    // do something
}
                    sh """
                      aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin   ${account_id}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${app_version} . 
                       docker images
                       docker push ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${app_version}
                       """
                }
            }
        }

        stage('SonarQube Analysis') {
            environment{
               def scannerHome = tool 'sonar-8.0' 
            }
            steps {
                script{
                withSonarQubeEnv('sonar-server') { 
                    sh "${scannerHome}/bin/sonar-scanner"
                }
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