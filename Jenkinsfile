pipeline {
    agent any
    // agent {
    //     label 'AGENT-1'
    // }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
        ansicolor('xterm')
    }
    environment {
        def appVersion = '' // declaring variable
        nexus_url = 'http://nexus.hellandhaven.xyz:8081/'
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
        stage('Sample test'){
            steps{
                script{
                    def appVersion = readJSON file: 'package.json'
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

        stage('upload artifact to nexus') {
            steps{
                script{
                    nexusArtifactUploader {
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexus_url}",
                    groupId: 'com.expense',
                    version: "${appVersion}",
                    repository: 'frontend',
                    credentialsId: 'nexus_auth', // manage jenkins - credentials - system - global cred 
                    artifact: [
                        artifactId: 'frontend',
                        type: 'zip',
                        classifier: '',
                        file: "frontend-" + "${appVersion}" + '.zip',
                        ]
                    }
                }
            }
        }
        stage('trigger deploy') {
            steps{
                script{
                    build job: 'frontend-deploy', propagate: false, wait: false,
                                parameters: [
                                   string(name: "appVersion", value: "${appVersion}"),
                                ]
                }
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
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