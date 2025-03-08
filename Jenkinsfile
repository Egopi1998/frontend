@Library('jenkins-shared-library') _

// create variable of map type and set the values

def configMap = [
    type: "nodejsEKS", // type of the project based on this type pipeline will trigger from shared library
    component: "frontend", // component of the project
    project: "expense"  // project name
]

if( ! env.BRANCH_NAME.equalsIgnoreCase('main')){
    pipelineDecission.decidePipeline(configMap)
}
else{
    echo "Proceed with CR or NON-PROD pipeline"
}