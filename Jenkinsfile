@Library('my-custom-library') _

def emptyMap = [
    project: "roboshop"
    component: "catalogue"
]

// if branch is not equal to main, then run CI pipeline
if ( ! env.BRANCH_NAME.equalsIgnoreCase('main') ){
    nodeJSEKSPipeline(configMap)
}
else {
    echo "Please follow the CR process"
}