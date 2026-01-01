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

            stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

         stage('Dependabot Security Gate') {
            environment {
                GITHUB_OWNER = 'HarinadhKamma'
                GITHUB_REPO  = 'catalogue'
                GITHUB_API   = 'https://api.github.com'
                GITHUB_TOKEN = credentials('GITHUB_TOKEN')
            }

        steps {
                script{
                    /* Use sh """ when you want to use Groovy variables inside the shell.
                    Use sh ''' when you want the script to be treated as pure shell. */
                    sh '''
                    echo "Fetching Dependabot alerts..."

                    response=$(curl -s \
                        -H "Authorization: token ${GITHUB_TOKEN}" \
                        -H "Accept: application/vnd.github+json" \
                        "${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts?per_page=100")

                    echo "${response}" > dependabot_alerts.json

                    high_critical_open_count=$(echo "${response}" | jq '[.[] 
                        | select(
                            .state == "open"
                            and (.security_advisory.severity == "high"
                                or .security_advisory.severity == "critical")
                        )
                    ] | length')

                    echo "Open HIGH/CRITICAL Dependabot alerts: ${high_critical_open_count}"

                    if [ "${high_critical_open_count}" -gt 0 ]; then
                        echo "❌ Blocking pipeline due to OPEN HIGH/CRITICAL Dependabot alerts"
                        echo "Affected dependencies:"
                        echo "$response" | jq '.[] 
                        | select(.state=="open" 
                        and (.security_advisory.severity=="high" 
                        or .security_advisory.severity=="critical"))
                        | {dependency: .dependency.package.name, severity: .security_advisory.severity, advisory: .security_advisory.summary}'
                        exit 1
                    else
                        echo "✅ No OPEN HIGH/CRITICAL Dependabot alerts found"
                    fi
                    '''
                    
                }
            }
        }

        stage('test'){
            steps{
                script {
                sh """
                npm test
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