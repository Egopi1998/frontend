pipeline {
    agent any
    // agent {
    //     label 'AGENT-1'
    // }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        def appVersion = '' // declaring variable
        nexus_url = 'nexus.hellandhaven.xyz:8081/'
    }
    stages{
        stage('Sample test'){
            steps{
                sh """
                    echo "this is testing "
                    ls -ltr
                """
                // ls -ltr is just to see all files successfully taken from git
            }
        }
        stage('appVersion'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version ${appVersion}"
                }
            }
        }
        stage('build') {
            steps{
                sh """
                    zip -q -r frontend-${appVersion}.zip * -x jenkinsfile -x frontend-${appVersion}.zip
                """
                // working Agent should have Zip. -x is for exclude
            }
        }
//         stage('Sonar scan') {
//             environment {
//                 scannerHome = tool 'sonar-6.0' //referering scanner cli
//             }
//             steps {
//                 script {
//                     withSonarQubeEnv('sonar-6.0') { //refering scanner server
//                         sh "${scannerHome}/bin/sonar-scanner" 
//                     }
//                 }
//             }
//         }
//         stage("Quality Gate") { //need to configure the webhook in sonarqube to get update from sonar server -- sonar - administration - configure - webhook - http://jenkins.hellandhaven.xyz:8080/github-webhook/
//             steps {
//               timeout(time: 30, unit: 'MINUTES') {
//                 waitForQualityGate abortPipeline: true //if quality gate fails pipeline aborts 
//               }
//             }
//         }
        stage('upload artifact to nexus') {
            steps{
                script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexus_url}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: 'frontend',
                        credentialsId: 'nexus_auth',
                        artifacts: [
                            [artifactId: 'frontend' ,
                            classifier: '',
                            file: 'frontend-' + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
        }

//         stage('trigger deploy') {
//             steps{
//                 script{
//                     build job: 'frontend-deploy', propagate: false, wait: false,
//                                 parameters: [
//                                    string(name: "appVersion", value: "${appVersion}"),
//                                 ]
//                 }
//             }
//         }
    }
    post { 
        always { 
            echo 'i am deleting the workspace'
            deleteDir() // this will delete workspace in agent 
        }
        failure {
            echo "you are seeing this because job is failed"
        }
        success  {
            echo "inkem undile panuko nuvv happy ga"
        }
    }
}